---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段のコード エディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat の agent mode を使い、Model Context Protocol (MCP) を通じて外部ツールに接続し、カスタム エージェントを活用します。統合開発環境 (IDE) を離れずに作業でき、Copilot はファイル、ターミナル、問題の全体を把握できます。

まずカスタム指示を追加し、Copilot がその指示に従う様子を確認します。次に agent mode で、ユーザー インターフェイス (UI)、データ層、テストにまたがるフィルタリング機能を構築します。続いて Playwright MCP サーバーを接続し、プル リクエストを作成する前に Copilot にブラウザーを操作させて機能をテストします。最後にアクセシビリティ作業用のカスタム エージェントを確認して使い、Copilot の変更を監視して指示を出し、反復して改善します。すべてエディターを離れずに進められます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して検証します |
| [2. Agent Mode][ex2] | コード生成 | agent mode でフィルタリング機能を構築します |
| [3. Playwright を使う MCP][ex3] | 外部ツール | Playwright MCP サーバーを使ってブラウザーで機能をテストします |
| [4. カスタム エージェント][ex4] | 特化型エージェント | カスタム エージェントを確認して使います |
| [5. エージェントの管理][ex5] | 監視 | エージェントのセッションを監視して指示を出します |
| [6. 反復して改善する][ex6] | レビュー | Copilot の作業をローカルでレビューし、次のステップを選びます |
| [任意: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit を使う 3 つのモジュールで、モデルを準備し、エージェントをデプロイしてサイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランの GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランをお持ちでない場合、条件を満たす学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、GitHub Copilot のコマンド ライン インターフェイス (CLI) 機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

## 開始する

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
