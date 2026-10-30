---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多交互用的斜杠命令。这些命令可以访问高级功能、查看幕后信息并提供额外配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在继续了解其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

Copilot CLI 核心工作流已经完成。现在了解其他能力，包括管理上下文、切换模型、共享会话，以及选择是否将任务委派给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或主动压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从仓库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口接近填满时自动压缩上下文。想自行选择时机时使用 `/compact`。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型及其使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员之间共享经验有助于共同改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享的链接，或发布为 GitHub gist。

1. 输入 `/help` 并查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择适合的目标，例如使用 `/share file` 在本地导出 Markdown 文件。
3. 发送给他人或发布前，请审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意共享的仓库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已完成。Copilot CLI 还为更大规模的任务提供了以下命令：

- `/fleet` 可以将独立子任务交给多个子代理并行处理。
- `/worktree` 可以为另一个任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给异步工作的 Copilot cloud agent，后者可能会创建拉取请求。

这些命令是可选的，因为它们可能会创建额外的工作树或远程任务。尝试前先创建一个范围明确的新任务，并通过正常工作流审查结果。若要深入了解异步代理工作方式，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并查看幕后情况。本课中你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和云委派的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。接下来[回顾所学内容][next-lesson]并了解后续步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
