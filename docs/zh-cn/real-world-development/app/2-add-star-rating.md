---
title: "第 2 课 - 添加星级评分：快速改进"
description: "在 GitHub Copilot app 中启动第一个代理会话，对游戏卡片进行小幅更改，并通过第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

浏览工作区并尝试快速聊天后，接下来启动**代理会话**，对项目进行第一次更改。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。你将请代理显示评分、审查更改并合并第一个拉取请求。

在本课中，你将：

- 启动代理会话并了解会话的结构。
- 请代理对项目进行一项范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示此评分。但主页上的游戏卡片只显示标题、类别、publisher 和说明。作为热身练习，你将让代理在每张卡片上显示现有评分。这项小型、独立的更改非常适合作为第一个会话任务。

## 会话的组成

**会话**是与代理进行的对话，在独立工作区中运行。每个会话都有专用的 **git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会造成更改冲突。会话按仓库分组显示在侧边栏中，选择任一会话即可切换。

会话中包含三类内容：与代理的**对话**、代理探索和编辑文件时执行的**工具操作**，以及带有差异的**已更改文件**列表。

## 启动会话并请求更改

启动新会话，开始探索项目并实现功能。在[应用设置][prior-lesson]期间，你已从 GitHub 仓库添加项目。现在为该仓库创建新会话并请求更改。

1. 返回或打开 GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 选择仓库 `tailspin-toys`。
4. 在提示框下方选择 **new working tree** 和 **Interactive** 模式，然后使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。然后它会探索项目，找到实现新功能所需更新的文件，并创建必要的代码。现在，你已使用 Copilot app 添加新功能。

## 审查差异

所有人工智能（AI）生成的更改在合并前都应接受审查，即使改动很小。现在直接在 Copilot app 中检查更改。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 查看 `GameCard.astro` 中新增的代码，该文件用于显示游戏详情。代码应与以下示例类似：有评分时显示评分，`starRating` 为 `null` 时显示 `No rating yet`：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果并非确定不变，因此实际代码可能与示例不同，但应大致相似。

## 检查更改

打开浏览器前，先审查代理自动检查的结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过检查不算通过；批准任何安装请求前，请先审查。

不能只阅读代码就假定它能正常运行。让 Copilot 启动网站，然后在浏览器画布中检查更新后的用户界面。

> [!TIP]
> 画布是 Copilot app 中的交互式小组件。稍后会探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示，请 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用将启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示了满分为五分的评分值。
4. 完成后，使用以下提示要求 Copilot 停止为此会话启动的开发服务器并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

功能已创建。接下来创建拉取请求（PR），将新代码合并到现有代码库。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser** 并按提示完成身份验证。
3. 如果 Copilot 说明仓库策略要求先关联 issue 才能创建 PR，请确认应遵循此流程。
4. 等待 Copilot 创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查窗格中打开拉取请求并查看内容。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并拉取请求。

## 总结和后续步骤

恭喜！你使用 GitHub Copilot app 发布了第一个更改。具体来说，你：

- 启动了代理会话并了解了会话的结构。
- 指示代理对游戏卡片进行一项范围明确的小改动。
- 在工作区差异视图中审查了更改。
- 在本地运行应用，并在浏览器中确认星级评分。
- 打开了第一个 PR、审查了检查结果并明确执行了合并。

接下来，从筛选 issue 开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [在 GitHub Copilot app 中使用代理会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
