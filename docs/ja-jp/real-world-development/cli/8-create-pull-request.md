---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成しましょう。最初のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示の更新、スキル、QA agent を複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質確認の一連の作業を 1 つの会話とブランチで進め、この PR にまとめます。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認します。
- 機能変更全体と検証の根拠を確認します。
- フィルタリング PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能を計画、実装、検証するために Copilot を使ってきました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プル リクエストをマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビュー コメントに対応し、必要に応じてリベースするなど、PR を妨げている問題に取り組みます。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をレビューして PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の CI を監視します。すべて成功すると、マージが実行されます。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コード生成、テストと検証、プル リクエストのプロセスなど、開発作業のいくつかを自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とマージ ライフサイクルを自動化する方法を確認しました。
- 機能変更全体と検証の根拠を確認しました。
- フィルタリング PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[Copilot CLI のスラッシュ コマンド][next-lesson]を確認し、コンテキスト、モデル、共有、cloud agent への任意の委任を試します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
