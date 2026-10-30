---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解如何完成。

在本练习中，你将：

- 了解如何通过仓库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码准则和文档规范。
- 在 `.github/copilot-instructions.md` 中添加一项新的全仓库规范。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用前后对比提示。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件产生的效果。

## 指令文件

### 场景

Tailspin Toys 有一套开发规范和要求：

- 数据层必须始终有单元测试。
- UI 应使用暗色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些规范，并按要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库以及偏好的注释类型，也可以为整个仓库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的指令文件。例如，为 TypeScript 或 Astro 提供语言指导，或为创建 UI 组件、单元测试提供任务指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用仓库级指令以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何创建指令文件的完整讨论超出了本工作坊的范围。示例项目展示了一种有代表性的做法：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例说明推荐和不推荐的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。查看本项目中的文件时，你可能会发现有些文件专门指导[更新 UI][ui-instructions]和 [Astro][astro-instructions]等任务。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作环境的相关课程会在需要时说明具体方法。
>
> 需要模板或起点？可以查看包含指令文件、自定义代理和其他资源的 [awesome-copilot][awesome-copilot] 仓库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义指令文件

请先查看仓库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于仓库根目录的 glob，用于确定指令适用的文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看针对本项目单元测试编写方式的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。路径相对于指令文件，而不是仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的仓库规范

接下来要做的唯一编辑，是添加一项全项目规则，要求代码中包含 TSDoc 文档注释和文件级注释。稍后的练习中，cloud agent 会在处理分配给它的 issue 时读取这些指令。

编辑前，先创建一个分支（cloud agent 会读取 issue 目标分支中的指令，但仍建议干净地提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，以改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。此处记录了项目编码标准，但尚无代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 行，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在仓库级文件 `.github/copilot-instructions.md` 中添加全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级规范，因此适合放在仓库级文件中。

## 提交、推送并合并指令更新

cloud agent 会从 issue 的目标分支读取指令。下一练习中，Copilot 会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一练习中分配 issue 后，cloud agent 就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 `.github/copilot-instructions.md` 中添加了一项全仓库规范。后续练习中，cloud agent 创建的拉取请求会体现这项规范。

接下来，[将第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
