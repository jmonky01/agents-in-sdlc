---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena atualização de uma aplicação pode exigir alterações em vários arquivos e tarefas como executar testes. Em geral, um desenvolvedor precisa localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar problemas, descobrir qual arquivo ficou de fora e atualizar o código novamente.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no ambiente de desenvolvimento integrado (IDE). Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Veja como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite ciclos de feedback rápidos e interação direta. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests para interagir com você.

Neste exercício, você verá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente do GitHub Copilot implementa recursos na interface do usuário (UI) e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, use o modo de agente do Copilot para concluir as alterações restantes na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender o comportamento atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. As pessoas podem consultar jogos e ver detalhes sobre eles. O site é uma única aplicação Astro que gera páginas estáticas durante a compilação usando a linguagem de marcação de hipertexto (HTML). As páginas consultam diretamente um banco de dados SQLite local pelo frontmatter, usando o mapeador objeto-relacional (ORM) Drizzle. Não há uma interface de programação de aplicações (API) de back-end separada nem um framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um pequeno elemento Astro `<script>` com APIs do modelo de objeto de documento (DOM).

### Iniciar o site

O script `npm run dev` inicia o servidor de desenvolvimento da aplicação Astro. Execute-o no GitHub Codespaces:

1. Volte ao Codespace. Após o exercício anterior, você deve estar em `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso com o endereço web (URL), semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal para abrir o site.

> [!NOTE]
> No Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para o servidor do Codespace.

### Explorar o site

Com o site em execução, explore as duas páginas principais:

1. Examine a **página inicial**, que mostra jogos de tabuleiro com títulos, imagens e descrições.
2. Selecione um jogo e examine a **página de detalhes**, que inclui título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

A implementação inicial do site funciona, mas você vai adicionar novos recursos. Comece examinando o backlog de issues, criado automaticamente quando você configurou o projeto a partir do modelo. Peça ao GitHub Copilot que mostre essas issues.

1. Na lista de agentes da exibição Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos do terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> A inclusão de **Claude Sonnet 4.5** não indica preferência dos autores por um modelo. Ele foi usado para criar este workshop e foi incluído para tentar manter as sugestões relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes das descritas. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt para pedir ao Copilot que examine o backlog:

   ```plaintext
   Mostre o backlog de issues do meu repositório do GitHub. Ajude-me a priorizá-las de acordo com o que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para permitir que o comando liste as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com a utilidade estimada para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, examine o arquivo de instruções que fornecerá contexto. O arquivo [de instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de criação de componentes, testabilidade e acessibilidade.
   - links para instruções específicas para Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações na aplicação:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes também devem ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e o arquivo `ui`.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da exibição Chat, confirme que **Agent** está selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente o recurso conforme a issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria, de acordo com os requisitos da issue relacionada no backlog do GitHub. O auxiliar de editoras já existe por causa do exercício anterior; preserve e aprimore esse trabalho conforme necessário enquanto conclui as alterações restantes na camada de dados, na interface e nos testes. Confirme que todos os testes passam antes de concluir. O servidor já está em execução, então não precisa iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relevantes da camada de dados, da interface e dos testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da tarefa e da base de código. Durante o processo, podem aparecer os controles **Keep** e **Undo** no editor. Aguarde o fim do trabalho; não é necessário selecionar esses controles enquanto o agente ainda está trabalhando.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do modo de agente no Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado e procure possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está em execução e explore o novo recurso.
11. Depois de confirmar que o recurso funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você viu como:

- implementar recursos na interface e na camada de dados.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados antes do merge.

Agora, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, pode ser necessário adicionar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções.
- decidir se as pessoas poderão escolher o tamanho da página ou se ele será fixo.
- fornecer contexto e orientações suficientes no prompt.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto adicional. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
