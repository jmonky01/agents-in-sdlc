---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、未完了のプル リクエスト (PR) があるエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同時に追加するのが自然です。エージェントに指示を出し、ハイ コントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用のアプリケーション プログラミング インターフェイス (API) を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、次の作業が表示されます。
   - リポジトリをチェックアウトする。
   - Copilot の作業環境を作成する。
   - Model Context Protocol (MCP) サーバーを設定する。
   - 割り当てられたタスクを完了するための各種操作を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインも確認します。ここでは、PR と変更されたファイルを確認できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェント タスクと、指示用ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を受け取り、作業に取り入れたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順が終わると、新しい依頼を作業に反映します。

この作業にも数分かかります。その間に、これまでに確認した内容を振り返ってください。

## まとめと次のステップ

Copilot agents ページを使って、Copilot cloud agent のタスクを監視し、ガイドする方法を確認しました。進行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のセッションに追加の指示を出せます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を出し、機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプル リクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
