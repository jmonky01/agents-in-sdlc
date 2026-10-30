---
title: "第 8 课 - 创建并合并功能拉取请求（PR）"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一并审查这些更改并创建拉取请求。你已自行合并星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理拆分为几个独立的 PR。为简化工作坊流程，筛选和质量工作流一直使用同一个对话和分支，所有更改都会放入此 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动执行合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后再启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动执行剩余的 PR 工作，同时由开发者控制合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻塞 PR 的问题，包括修复失败的持续集成（CI）检查、回应审查意见，以及在需要时变基；之后，如果仓库允许，就会启用 GitHub 自动合并。

此前，你一直自行选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码完成后，一起审查这些更改、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 将监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、仓库设置或权限限制。如果流程受阻，请阅读报告的原因，并在仓库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动执行开发流程中的多个环节，包括生成代码、测试和验证代码，以及拉取请求流程。本课中你：

- 了解了 Agent Merge 及其如何自动执行合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
