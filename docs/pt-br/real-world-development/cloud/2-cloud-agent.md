---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Instruções personalizadas][previous-lesson]

Muitas organizações enfrentam dívida técnica, como problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) de rascunho para revisão por um desenvolvedor. Assim, você pode delegar tarefas repetitivas e se concentrar em questões mais abrangentes.

Neste exercício, você vai explorar como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer lidar com parte da dívida técnica. A equipe contratada para criar a primeira versão do site deixou a documentação em um estado inadequado: na prática, ela está ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação de TypeScript (TSDoc) às funções exportadas da aplicação.

A equipe de design também quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria para que as pessoas continuem explorando o catálogo. A equipe não precisa de uma implementação pronta para produção, mas quer algo que possa usar em testes de aceitação da experiência do usuário (UX). Esse trabalho está bloqueado por outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser facilmente adiadas e que são adequadas para o agente de nuvem. O agente trabalha de forma assíncrona enquanto você se dedica a outras tarefas e, depois, revisa os resultados.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a um desenvolvedor. Você pode iniciar o trabalho, por exemplo, [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request (PR) de rascunho para acompanhar o progresso, prepara o ambiente e começa a trabalhar. Você pode acompanhar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot marcará você no PR.

## A importância de instruções com escopo bem definido

O GitHub Copilot não elimina a necessidade de pensar. Até operações aparentemente simples podem ter complexidades. [Defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adição de IA generativa.

## Instruções personalizadas neste repositório

Exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes, sem alterá-los, para depois identificar sua influência.

Abra os arquivos a seguir na interface web do GitHub ou em um Codespace já iniciado para revisar as instruções antes de atribuir trabalho:

1. Abra `.github/copilot-instructions.md` e examine a seção **Code standards**, principalmente as orientações para TypeScript e comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários no PR. Essas orientações vêm dos arquivos de instruções; você as examinará novamente ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código geralmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml` e executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências necessárias. O modelo do laboratório já inclui esse workflow para facilitar o exercício. Ele configura o Node.js, as dependências do projeto, os navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para a aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (agora, todo o aplicativo usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele tem um único job, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar o Copilot iniciar o trabalho.

## Adicionar documentação

Muitos projetos têm documentação desatualizada ou inexistente. Essa dívida técnica pode reduzir a produtividade, dificultar a manutenção e complicar a integração de novos desenvolvedores. O Copilot pode ajudar a criar documentação, então esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano e você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o PR recém-criado, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer. O Copilot está preparando o ambiente com o workflow descrito acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o agente planeja concluir.
14. Role a linha do tempo do PR para baixo e localize a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Acompanhe a sessão em andamento e observe como o Copilot explora o código, entende o estado do projeto, define uma abordagem e cria código.

O trabalho pode levar vários minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona, liberando você para outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, trabalhe em um recurso da aplicação.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é poder dividir o trabalho: você cuida de uma tarefa enquanto o agente cuida de outra. A seção de jogos relacionados para a equipe de design talvez não demore muito, mas esse tempo também pode ser usado em outras atividades. Atribua essa tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá olhos na primeira observação da issue, indicando que o Copilot começou a trabalhar.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma assíncrona, você não precisa acompanhá-lo o tempo todo; revise o resultado quando estiver pronto. Enquanto isso, conheça os agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona e delegar tarefas de dívida técnica, novos recursos ou migrações entre frameworks.

Você explorou como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e instruções personalizadas, apresentadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
