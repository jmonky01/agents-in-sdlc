---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar cada diff na conversa, aceitar ou rejeitar alterações individuais e orientar o agente com prompts complementares sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle do modo claro além do alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design pediu também um controle de modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A exibição do Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição do Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal solicitados.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Selecione **Files changed** na parte superior do chat para examinar todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir autorização, por exemplo, para executar um comando do terminal, o agente pausará e mostrará a opção **Allow**. Aprove somente os comandos que você considerar seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar um controle do modo claro. Como você continuará na mesma sessão, o agente mantém o contexto do trabalho, incluindo os componentes alterados, a abordagem de estilo e o uso de armazenamento local.

1. Na caixa de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Bom trabalho! Agora adicione um controle do modo claro ao lado do controle de alto contraste. Siga o mesmo padrão: persista a preferência no armazenamento local e disponibilize o controle na mesma área da interface. As pessoas devem poder ativar o alto contraste e o modo claro de forma independente.
   ```

2. O agente retomará o trabalho, explorará as alterações existentes e proporá adições para o modo claro.
3. Revise cada nova edição e selecione **Keep** ou **Undo**, como fez para o modo de alto contraste.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar os controles de uma maneira que você não quer — selecione **Stop** ao lado da caixa de entrada e envie uma orientação complementar mais específica.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes, corrigir o rumo ou ampliar o escopo. Por exemplo: simplificar o CSS usando variáveis em vez de duplicar seletores; integrar o modo claro ao painel de configurações existente, sem adicionar outro controle; ou adicionar atalhos de teclado aos dois controles.

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente todos os arquivos alterados pelo agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente personalizado aparecem nas alterações, incluindo atributos de Aplicações Ricas de Internet Acessíveis (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
