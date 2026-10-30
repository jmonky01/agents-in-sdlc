---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente que têm pull requests (PRs) abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir a implementação do modo de alto contraste, a equipe percebeu que também seria útil adicionar um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, você vai orientar o trabalho para incluir o modo claro além do modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Verifique o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel esquerdo. Entre elas, devem estar:
   - atualização da documentação da base de código.
   - geração de APIs para modificar jogos.
   - adição de um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - preparar o ambiente de trabalho.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request no lado direito, que mostra o PR e os arquivos alterados para facilitar o monitoramento.

## Orientar o agente de nuvem

Agora que você examinou as tarefas ativas, peça ao Copilot que adicione um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens indicando o início das alterações.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o pedido e o incorpora ao fluxo de trabalho.

## Aguardar o Copilot concluir o trabalho

O Copilot incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como isso pode levar alguns minutos, aproveite para refletir sobre o que você já aprendeu e explorou.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o ponto central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nesse painel, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento para alterar o escopo ou fornecer orientações.

Você explorou como:

- usar o painel central e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho de acessibilidade, você [iterará sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
