---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "查看仓库指令，添加文档规范，并将新规范应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能访问相关指导。[指令文件][instruction-files]不仅说明希望生成什么代码，也说明代码应如何组织。现在筛选功能已经构建完成，可以查看 Copilot 使用的指令、添加文档规范并将其应用到代码中。

在本课中，你将：

- 了解仓库指令和按路径应用的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发规范：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指导。

通过指令文件，可以确保 Copilot 了解这些规范，并按要求完成工作。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库以及偏好的注释类型，也可以为整个仓库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件，应包含大多数请求适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的指令文件。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看本项目的自定义指令文件

项目模板已经附带一组指令文件。更改代码前，先查看现有文件，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件中的指令适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他指令文件和项目现有文件的链接。这样可以拆分较大的指令集、复用内容，并提供 Copilot 可参考的示例。

## 更新指令文件以符合团队规范

现有文件已经提供了良好的起点，但仍有缺失。接下来修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 将以下内容添加为该部分的最后一个项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载仓库指令。保存更新后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的仓库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认已检出 `game-filters-cli` 分支。
3. 使用以下提示，请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何从指令文件中获取上下文，并将新规范应用到功能代码中。具体来说，你：

- 了解了仓库指令和按路径应用的指令文件如何传递给代理。
- 更新了指令文件以确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
