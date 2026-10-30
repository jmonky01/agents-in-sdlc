---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面对技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 的 cloud agent 可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给代理处理，加快开发进度，让开发者专注于更重要的工作。

你将使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 希望解决一些技术债务。最初受聘开发网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望每个游戏详情页都显示相关游戏，也就是同一类别中的其他游戏。当前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项工作目前受阻，但现阶段有其他优先级更高的 issue。

这两项任务都可能很快被推迟，因此适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者继续其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，深入分析后也会发现其复杂性。因此，[为 Copilot cloud agent 明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为人工智能（AI）结对编程助手：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本仓库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的指导作用。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看仓库中的指令文件，稍后便能观察它们的影响。

在仓库的 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` issue 分配给 cloud agent 时，留意它创建的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件头注释。这些指导来自上述指令文件。后续练习审查拉取请求时还会再次查看。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 也一样，它会像软件工程师一样处理工作。

cloud agent 使用 [GitHub Actions][github-actions] 环境执行任务。你可以在 `.github/workflows/copilot-setup-steps.yml` 中配置[专用设置工作流][setup-workflow]，在代理开始工作前运行，以便安装所需的开发工具和依赖项。练习仓库已预先配置此工作流，以确保流程顺利并提供实践机会。它会为单体 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在仓库的 "Actions" 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆仓库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆仓库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro 和 Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在仓库的 **Actions** 选项卡中手动运行工作流。这样可以先测试工作流是否成功，无需等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但许多项目的文档已经过时，或者完全缺失。这类技术债务往往无人处理，会拖慢开发效率、增加代码库维护难度，也让新开发者难以加入团队。Copilot 擅长生成文档，因此很适合将这类 issue 分配给 Copilot cloud agent。它会在后台生成所需文档，之后可以回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办事项和 Copilot 计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并选择合适计划，以及编写代码。

此过程可能需要几分钟。Copilot cloud agent 的主要用途之一是异步执行任务，让开发者有时间处理其他工作。接下来会利用这一点：先给 cloud agent 分配另一项任务，然后开始为应用编写功能代码。

## 为游戏详情页添加相关游戏部分

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组任务。为设计团队添加相关游戏部分可能不需要很长时间，但仍可以把这段时间用于其他工作。现在将此任务分配给 Copilot cloud agent。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

之后，你会在 issue 的第一条评论旁边看到 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。cloud agent 的工作方式类似软件工程师，因此无需一直主动监控；任务完成后再审查即可。现在可以开始了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将 issue 分配给 cloud agent，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

cloud agent 正在后台工作。接下来了解如何创建和使用自定义代理。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
