---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem predefinida.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e integrou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor do Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge no PR de filtragem.
- explorou comandos de barra para gerenciar contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas:** exibir o `starRating` existente e um estado explícito sem avaliação nos cards dos jogos.
2. **Fluxo de filtragem e qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório da skill `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Desde o planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: com testes automatizados, uma inspeção própria no navegador e a exploração do Copilot por meio do MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados e avaliou os requisitos e a cobertura dos testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que não são necessários testes novos pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após mudanças.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot orientam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para avaliar uma abordagem antes de desenvolver, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e outro mais avançado para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* influencia significativamente o resultado.

## Mais recursos para explorar

Você concluiu o fluxo principal. Conheça outros recursos da CLI:

- `/review` pede ao agente de revisão de código que analise as alterações.
- `/rubber-duck` ajuda a analisar um problema e obter outra perspectiva.
- `/fleet` coordena tarefas independentes em paralelo.
- `/worktree` isola outra tarefa.
- `/delegate` envia uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de aprimorar o uso de uma ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você planeja desenvolver há anos. Compartilhe o que aprendeu com a equipe e aprenda com as experiências dela. E, como sempre, explore a documentação.

Para ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
