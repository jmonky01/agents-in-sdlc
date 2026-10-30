---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的仓库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于代理式开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和 CI 管道开箱即用。它面向同时指挥多个代理的工作流，每个代理都在独立工作区中运行；你无需亲自完成所有工作，也可以自动处理重复任务。Node.js 已安装，项目副本也已准备好，接下来安装应用并连接仓库。

在本课中，你将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 仓库将项目添加到应用。
- 熟悉工作区，包括模板为你预置的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正采用人工智能（AI）代理处理不断增长的待办事项。Copilot app 提供统一的工作位置，可领取 issue、运行代理、审查更改并合并拉取请求。本课将指导你完成安装和连接，并熟悉如何针对项目开始对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

首先安装 GitHub Copilot app。应用支持 Windows、macOS 和 Linux。接下来安装应用、完成身份验证并添加 Tailspin Toys 仓库。

1. 在浏览器中打开 [GitHub Copilot app 产品页][download-app]。
2. 下载适用于当前平台的应用，并按照产品页上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果初始设置流程提供仓库选择，请选择从模板创建的 Tailspin Toys 仓库。也可以跳过仓库选择，并在完成设置后添加项目。
6. 按提示选择主题，然后完成初始设置。
7. 在侧边栏的 **Sessions** 下查找仓库。如果已列出，则无需再次添加。
8. 如果尚未连接仓库，请选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和仓库名称。
9. 确认仓库出现在侧边栏中。此布局会将 `tailspin-toys` 等仓库文件夹归在 **Sessions** 下，每个仓库下方列出单独的会话。

## 熟悉工作区

连接项目后，花一点时间熟悉工作区。应用将功能组织在侧边栏的几个区域中：

- **New**：在这里启动与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示拉取请求。你可以浏览和筛选拉取请求、查看持续集成（CI）状态并审查拉取请求（PR），无需离开应用。
- **Issues**：以同样的方式显示 issue。你可以浏览和筛选 issue，并从 issue 启动会话。
- **Automations**：按计划或按需运行的已保存代理任务，适合管理待办列表、定期项目维护或其他重复工作。总结课程会将其作为后续探索方向，而不是另一项工作坊练习。
- **Customize**：通过模型上下文协议（MCP）服务器、插件、技能和其他组件为 Copilot app 添加功能。你将使用它配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需单独创建分支或工作区。开始第一次聊天后，此部分才会显示。如果未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按仓库文件夹归类会话和代理会话。选择 `tailspin-toys` 等仓库名称可打开仓库视图；选择仓库下的会话可返回对应对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话而不会发生更改冲突。添加星级评分时会启动第一个会话。

完成工作坊的过程中，你会逐步探索工作区。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作，或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找模板预置的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在查看通过模板创建项目时生成的待办事项。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 仓库文件夹，而不是其下方的单个会话。然后在仓库顶部导航中选择 **Issues**。
2. 按标题查找以下 issue，不要假设它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个 issue 并阅读其详情。每个 issue 也可以作为代理会话的启动点。完成第一次快速更改后，将从筛选 issue 启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的仓库中的项目。若要查看其他仓库的工作项，请将其添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是用它来了解*应用本身*。**快速聊天**正适合此用途。快速聊天无需创建分支或工作树即可提问或集思广益，因此适合快速、一次性的问题，无需启动会话。

1. 在侧边栏中，选择 **Chats** 旁的 **+**，打开新聊天。如果尚未显示 **Chats**，请选择 **Sessions** 旁的 **+** 开始第一次聊天；之后会显示 **Chats**。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 git 工作树中运行，因此可以并行运行多个代理而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

你已安装 GitHub Copilot app、连接项目并熟悉工作区。你了解了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中查找模板预置的待办事项。
- 使用快速聊天提出快速的一次性问题。

接下来[启动第一个代理会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [开始使用 GitHub Copilot app][getting-started]
- [在 GitHub Copilot app 中使用代理会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
