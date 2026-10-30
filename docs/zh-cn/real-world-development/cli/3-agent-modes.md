---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚才为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流程，帮助以正确方式构建正确的功能。本课将开启一系列课程，按照典型的代理式开发流程，从 issue 出发生成新功能、验证代码、确认功能行为，最后成功合并到项目中。

> [!NOTE]
> 后续功能开发会继续使用同一对话和分支。通常，不同文件类型的更改会使用不同分支或 PR；此处为聚焦核心概念而简化流程。

在本课中，你将：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程会继续完善此功能：更新仓库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和 publisher 筛选游戏。待办 issue 已描述该功能，但组合类别等细节需要在编写代码前达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则，实际上这些原则变得更加重要。大多数开发者的流程类似于：

1. 从描述需求的 issue 开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 不同团队和组织的具体流程会有所不同，但大多与上述流程类似。

遵循这种标准方法，可以确保人工智能（AI）生成的代码符合要求，并经过与手工编写代码相同的验证流程。

## 对话模式

**对话模式**决定代理具有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并迭代，直到认为任务完成。

先使用 Plan 模式，审查计划后再使用 Autopilot 实现。

## 从 issue 开始

开始筛选功能前，返回 Codespace，确认仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

注意提示现在以 `#7` 或类似编号开头。`#` 可将 GitHub issue 或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划能让你在开始实现功能或执行任务前确定实施方案。对于复杂任务，花些时间规划很有价值。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定最佳方案，以制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划完成并审查后，就可以开始实现。使用 Autopilot 让 Copilot 按计划执行。

Autopilot 会让 Copilot 持续迭代问题，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，合并前应像审查其他代码一样审查它。检查代码并运行网站，确认功能符合预期。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据 issue 和规划阶段确定的决策审查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但是否能正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按 publisher 筛选，以及按计划确定的组合方式筛选。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中你：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义指令][next-lesson]深入了解如何生成遵循既定实践的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
