---
title: "レッスン 7 - 品質保証 (QA) agent を作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーのフィルタリングを確認しました。次に、明確な QA プロセスを持つカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA agent を選択し、フィルタリング issue に照らして報告内容を確認します。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントで QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには、目的、指示、使用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件の評価と結果報告にこれらをどう使うかを Copilot に伝えます。既存の機能を置き換えるものでも、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. 既定のエージェントに次のプロンプトを送信して、新しいカスタム エージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

QA agent を使う前に、意図した QA ワークフローと境界がプロファイルに反映されていることを確認します。これにより、検証だけを依頼したのに機能を変更するような、不完全または広すぎるエージェントの使用を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、設定すると一覧で識別しやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. これらの責務や境界が欠けている場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後に同じフィルタリング会話を再開し、QA を選択して、issue と計画のコンテキストを使えるようにします。

1. 次のコマンドを入力して QA agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成した直後は、使用可能なエージェント一覧に表示されない場合があります。上記のコマンドで有効化できます。

2. 次のプロンプトを送信して、機能をレビューするよう QA agent に依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を開始します。
4. 完了したら、報告内容を読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに加え、結果を確認しました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らして報告内容を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
