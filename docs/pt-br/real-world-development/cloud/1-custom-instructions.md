---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa deve ser feita de uma maneira específica ou o Copilot precisa conhecer informações adicionais, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] orientam o Copilot não apenas sobre *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo por caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não fará uma comparação de prompts antes e depois. O agente de nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, então é mais difícil demonstrar o efeito lado a lado. Você verá a influência das instruções ao revisar os pull requests gerados pelo agente de nuvem.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para práticas de desenvolvimento:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação do TypeScript (TSDoc).
- O início de cada arquivo deve conter um bloco de comentários que explique sua finalidade.

Os arquivos de instruções ajudam a garantir que o Copilot tenha as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos de código. Você pode especificar convenções, bibliotecas e comentários preferidos. As instruções podem valer para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto aplicável à maioria das interações com o Copilot, como a stack, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que fornecem orientações para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções orientam a geração de código no Copilot Chat, mas não as conclusões de código nem as sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita arquivos de instruções em outros formatos][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem representativa para organizar arquivos de instruções:

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como descrição, estrutura e padrões globais de código.
- Use arquivos `*.instructions.md` para tipos de arquivo, como testes de unidade, componentes Astro e camada de dados, ou para tarefas específicas.
- Escreva em linguagem natural, com orientações claras e exemplos de código desejável e indesejável.

Não há uma única maneira certa de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona melhor para o projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot podem se beneficiar de uma coleção consistente de arquivos de instruções. Neste projeto, há orientações para [atualizações de interface][ui-instructions] e [Astro][astro-instructions], entre outras tarefas.
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa funcionalidade de uma maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O percurso correspondente explica o recurso quando necessário.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório: um arquivo central `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas específicas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre workflows do GitHub Actions. Essas instruções podem se aplicar a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um padrão glob relativo à raiz do repositório; neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Os caminhos são relativos ao próprio arquivo de instruções. Isso permite dividir instruções extensas em arquivos menores e reutilizáveis e indicar exemplos para o Copilot.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão ao repositório

Agora, você fará uma alteração: adicionará uma regra geral para documentar o código com comentários TSDoc e um cabeçalho de comentário por arquivo. O agente de nuvem usará essa regra nas issues atribuídas mais adiante.

Antes de editar, crie uma branch. O agente de nuvem lê as instruções da branch de destino da issue, mas você fará um commit organizado das alterações.

1. No terminal do Codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela ainda não exige documentação no código.
4. Logo abaixo dos padrões existentes, adicione estas linhas de Markdown:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Os arquivos de instruções podem ser criados no nível do repositório, em `.github/copilot-instructions.md`, para orientações gerais, ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicos. O arquivo do repositório é adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, push e merge da atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ao atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, as instruções precisam estar em `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá usar as instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa de branch e fazer o commit diretamente em `main`. A branch serve para reproduzir o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções do projeto e adicionou um novo padrão a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem gerar neste percurso.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
