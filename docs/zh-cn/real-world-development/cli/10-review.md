---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、熟悉了项目并找到了预置的筛选 issue。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案、使用 Autopilot 模式构建，并在 Interactive 模式中审查。
- 使用自定义指令指导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中检查筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 你交付的内容

本工作坊包含两个 PR 里程碑，每个都从已更新的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新并应用指令、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化工作坊流程，这些工作都合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中体验功能。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 代理将这些结果与需求审查和测试覆盖范围结合起来，再供 PR 审查使用。

新增测试应解决真实覆盖缺口；如果 QA 检查后不需要添加测试，也可能是正确结果。授权合并前，请审查代码和证据。更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复任务，自定义代理定义专家角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进聚焦的更改，使用 **Autopilot** 处理范围明确的任务。日常修改可选择速度较快的模型，复杂任务则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*实现，会显著影响生成结果。

## 继续探索

核心工作流已完成。还可以了解一些 Copilot CLI 功能：

- 使用 `/review` 请求代码审查代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离其他任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

熟练使用工具的最佳方式就是持续使用它。可以将 Copilot 用于生产代码、个人项目，或一直想构建却没有动手的小应用。与团队分享经验、相互学习，并继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，请查看 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
