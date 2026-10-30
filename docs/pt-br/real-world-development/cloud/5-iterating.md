---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade, incluindo controles para alternar entre os modos de alto contraste e claro. Agora, examine as alterações no código e, se necessário, dê feedback ao Copilot para aprimorá-las.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inclusão de IA generativa. É importante revisar o código gerado e seguir o processo normal de DevOps. Revise as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de disponibilizar o trabalho para revisão da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão contínua, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem trabalha no GitHub Actions, em um ambiente separado e temporário.
- Um desenvolvedor precisa aprovar a execução de workflows do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Comece examinando o primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot, que adiciona documentação ao código. Use a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Acesse seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que pode incluir novos comentários de documentação do TypeScript (TSDoc) e outros tipos de documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários, orientados pelos arquivos de instruções personalizados que você examinou no início do Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Acompanhe os workflows na seção de verificações do PR. Se tudo correr bem, as verificações do projeto para a aplicação Astro passarão. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários inline no código, como faria com outra pessoa da equipe. O Copilot verá o comentário e iniciará uma nova sessão para tratar o pedido. Como os resultados não são determinísticos, não há um texto único a usar. Por exemplo, você pode pedir ao Copilot que:

- adicione no início de cada arquivo de código um cabeçalho com uma breve descrição de sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura da aplicação Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra sugestão de documentação que queira ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, verifique se há um novo commit no PR.
5. Selecione a guia **Files changed** para revisar as mudanças.

6. Se o resultado ainda precisar de ajustes, continue a iteração até ficar satisfeito.
7. Quando terminar, marque o PR como pronto para revisão e integre-o à branch principal.

   ![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Revise o PR de jogos relacionados para confirmar que o recurso atende aos requisitos e que as verificações foram concluídas.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
6. Verifique se há um indicador de workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Acompanhe os workflows na seção de verificações do PR. Se tudo correr bem, as verificações do projeto para a aplicação Astro passarão. Isso pode levar alguns minutos.
9. **Opcional:** alterne para a branch do Copilot no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot implementou o recurso de jogos relacionados. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas.

10. Se quiser continuar iterando, adicione um comentário na guia **Conversation** solicitando um ajuste, como alterar a quantidade de jogos exibidos ou adicionar cabeçalhos e comentários TSDoc. O Copilot iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel central no Exercício 4.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** e revise o código gerado.
5. Examine a implementação, com atenção especial a:
   - componentes da interface para alternar os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de folhas de estilo em cascata (CSS) ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos de Aplicações Ricas de Internet Acessíveis (ARIA) e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Acompanhe os workflows na seção de verificações do PR. Se tudo correr bem, as verificações do projeto para a aplicação Astro passarão. Isso pode levar alguns minutos.
10. **Opcional:** alterne para a branch do Copilot no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie a aplicação e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências permanecem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade.

11. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional: continue delegando

O agente de nuvem é útil quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para praticar, escolha uma ideia e atribua a tarefa correspondente ao Copilot:

1. Escolha uma tarefa: criar um formulário de interesse de apoiadores na página de detalhes do jogo, implementar paginação na lista de jogos ou adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.
2. Crie uma issue que descreva a tarefa escolhida.
3. Atribua a issue ao Copilot.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para ver como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, incluindo controles dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar uma sessão de acessibilidade em andamento.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou:

- [**Percurso do VS Code**](../../vscode/): explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- [**Percurso da CLI**](../../cli/): entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

Para continuar praticando, escolha uma destas ações:

1. Atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
2. Crie um agente personalizado para outro domínio.
3. Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
