---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本工作坊中，你与 GitHub Copilot 合作完成了多项任务，以改善用户体验并添加功能：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在审查代码更改，并在需要时向 Copilot 提供反馈，改进其工作成果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再为团队启用代码审查。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会在没有监督的情况下异步执行任务，因此它受到一些安全限制：

- Copilot 对仓库只有读取权限，对其工作分支才有写入权限。
- cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），其中包含代码文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息一下，或回顾已学内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释。这些内容来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查列表。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果并非确定不变，因此无法提供固定的提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并提及 **@copilot**，请求修改生成的文档。可以使用上面的建议，也可以提出其他代码文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认结果后，可以将 PR 从草稿转换为准备审查状态，再合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 创建的 PR，查看解决游戏详情页显示相关游戏 issue 的更改。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求的检查列表。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。和之前一样，可以继续迭代并请求 cloud agent 更新。例如，可以调整显示的相关游戏数量，或要求添加文件头注释和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令）。如前所述，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。这个 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点检查：
   - 用于切换模式的用户界面（UI）组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表（CSS）或样式更改。
   - 无障碍富互联网应用（ARIA）属性、键盘导航等。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查列表。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

cloud agent 最适合处理真实待办事项，让你能专注于其他工作。可以在自己的仓库中创建几个 issue 并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作环境的练习。在这些课程中，你：

- **检查了仓库附带的自定义指令文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**来处理无障碍任务，添加高对比度和浅色模式切换功能。
- **使用 Copilot agents 页面作为任务控制中心**，监控并指导正在进行的无障碍会话。
- **审查并迭代改进 cloud agent 的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作环境的练习。还可以继续探索其他工作环境，补充本次实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中直接体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接功能。

也可以在自己的仓库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
