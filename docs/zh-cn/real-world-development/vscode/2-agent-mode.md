---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发者往往需要查找相关文件、进行修改、运行测试、调试、找出遗漏文件并再次更新，过程可能持续很久。

Copilot 代理模式可以帮助处理这些工作。

Copilot 代理模式可以在 IDE 中更自主地工作。它会先探索现有项目结构，查找需要修改的内容，执行必要的更新并运行测试，还会自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、查找相关文件并协调多文件更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 合并代码前，如何审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和 publisher 筛选游戏。上一练习已添加 publisher 辅助函数；现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先查看 Tailspin Toys 网站，了解现有功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，可列出游戏并显示其详情。它是一个单体 Astro 应用，页面在构建时渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则通过精简、限定范围的 Astro `<script>` 和标准文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，此时 Web 服务器正在 codespace 中运行。

### 浏览网站

网站运行后，可以查看其现有功能：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、publisher 和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的会话。每个练习会在相关步骤说明模式和模型选择方式。

网站的初始版本可以正常使用，但还可以添加新功能。先查看待办事项列表。通过模板设置项目时，系统已自动创建 issue；请让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。该代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非在推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便尽量保持体验一致。不过，大型语言模型（LLM）的结果并非确定不变，你收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于结果存在概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问仓库的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

Copilot 还会根据它认为对用户最有用的事项为列表排序。

## 查看指令文件

让代理生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看文件中添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型具体说明的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以复用内容、拆分大型指令集，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新三个部分：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以完成这些工作。

1. 可以继续当前 Copilot 会话，也可以选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，添加 UI 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍然选中 **Agent**。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能有关的文件。它应会查找数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。工作尚未完成时无需选择；Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框截图，询问是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理过程中可能会暂停，测试也可能暂时失败。这是正常的。Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式已完成的 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜你！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、查找相关文件并协调多文件更改。
- 合并代码前，如何审查 Copilot 代理模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 奖励探索练习 — 实现分页

随着游戏列表增长，网站将需要分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。编写提示和实现时，可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供 Copilot 所需的指导，确保实现符合预期。
- 可能需要多次迭代，要求 GitHub Copilot 修改并补充上下文。这是与 Copilot 协作的正常过程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
