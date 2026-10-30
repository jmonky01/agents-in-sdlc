---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 承担了三个独立任务，用于改善用户体验并添加功能。cloud agent 虽然可以异步、自主地工作，但仍需要监控任务。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心中，可以查看所有拥有未关闭拉取请求的代理任务、了解它执行的操作，还可以指导正在进行的会话。

在本课中，你将：

- 浏览代理页面并监控 cloud agent 任务。
- 指导正在进行的会话添加其他功能。

## 场景

将任务分配给代理以实现高对比度模式后，团队意识到也可以顺便添加浅色模式。由于代理正在更新网站样式并添加切换功能，因此同时加入浅色模式很合理。你将指导代理，使其在高对比度模式之外也实现浅色模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成所分配任务所需的其他步骤。

> [!NOTE]
> 根据 Copilot 的工作状态和执行方式，显示的具体步骤会有所不同。

4. 同时查看右侧显示的拉取请求窗格。你可以在此查看拉取请求和已更改的文件，以便进一步监控。

## 指导 cloud agent

查看当前任务后，接下来要求 Copilot 在实现高对比度模式时，也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到代理完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的 cloud agent 任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 对提示的确认，以及它如何将新要求纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。完成你发送消息时正在执行的步骤后，它会将新要求纳入工作流程。

这同样需要几分钟。等待期间，可以回顾目前学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和指导 GitHub Copilot cloud agent 任务的中心位置。通过任务控制中心，可以跟踪所有正在进行和已完成的任务、审查代理所做的工作，还可以调整正在执行的任务或提供额外指导。

你了解了以下内容：

- 使用任务控制中心和代理页面监控 cloud agent 任务。
- 指导正在进行的会话添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
