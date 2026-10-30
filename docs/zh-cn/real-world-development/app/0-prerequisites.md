---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的仓库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，也是使用 Copilot 和 GitHub 的中心位置。它可以快速访问 issue 和拉取请求，也支持使用 GitHub Copilot 开发。在本工作坊中，你将在本地使用 GitHub Copilot app 更新基于 Astro 构建的 Tailspin Toys 应用。开始前先确认本地已安装 Node.js，然后再安装 Copilot app。

在本课中，你将：

- 安装 Node.js，以便在计算机上运行项目测试。
- 根据模板创建自己的 Tailspin Toys 项目副本。

## 安装 Node.js

多节课程会要求代理构建功能，并在本地运行 Tailspin Toys 测试套件，这需要项目唯一依赖的运行时 **[Node.js][nodejs]**。请安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中使用 Windows Terminal、macOS 终端或常用工具打开终端窗口。
2. 运行以下命令，检查已安装的 Node.js 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两条命令都成功，并且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node 或需要更新时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保持选中的 **Add to PATH** 选项。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装：

    ```bash
    node --version
    npm --version
    ```

9. 终端应显示已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 代理也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功，并不代表应用一定可以访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍有命令无法运行，请按[会话环境故障排除步骤][session-environment]操作后再继续。

> [!IMPORTANT]
> 每个工作树还需要项目依赖项和用于端到端（E2E）检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 仓库 README，并在批准前审查安装请求。

## 设置练习仓库

你将在自己的 Tailspin Toys 项目副本中工作。现在从[模板仓库][template-repository]创建副本。新仓库包含练习所需的所有文件，安装应用后会连接该仓库。

1. 在新的浏览器窗口中打开本练习的 GitHub 仓库：`https://github.com/github-samples/tailspin-toys`。
2. 在练习仓库页面上选择 **Use this template**，然后选择 **Create a new repository**。

    ![展开 Use this template 下拉菜单并选中 Create a new repository](../../../_images/app-0-use-template.png)

3. 如果你参加的是由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新仓库。

    ![Create a new repository 表单，其中 github-samples/tailspin-toys 被设为模板，且已填写仓库名称](../../../_images/app-0-create-repository.png)

4. 记下所创建仓库的路径（**organization-or-user-name/repository-name**），稍后练习中会用到。

> [!NOTE]
> 从模板创建仓库时，系统会自动创建一组 GitHub issue。整个工作坊都会使用这些 issue，无需自行创建。

请使用工作坊模板的新副本。其中包含仓库指令、应用代码、测试、quality-checks 技能和现有画布扩展。你将在工作坊中自定义该技能，并创建 QA 代理。如果使用旧副本，请向活动组织者确认其中包含所需文件。

## 总结和后续步骤

环境已准备就绪。本课中你：

- 安装了 Node.js，以便在本机构建和测试项目。
- 根据模板创建了自己的 Tailspin Toys 仓库副本。

接下来[安装 GitHub Copilot app][next-lesson]、连接刚创建的仓库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建仓库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从-issue-启动会话
