---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトとシード済みのフィルタリング issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan mode で方法を決め、Autopilot mode で実装し、Interactive mode でレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証根拠を評価しました。
- フィルタリング変更全体をレビューし、Agent Merge を使ってフィルタリング PR を承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 提出した変更

ワークショップには、それぞれ更新済みの `main` から作成するブランチ上で進める 2 つの PR があります。

1. **星評価:** ゲーム カードに既存の `starRating` と、評価がない状態を明示して表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめました。

## 検証方法の違い

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA はこれらの結果と、要件およびテスト カバレッジのレビューをまとめて PR の確認に使いました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証結果をレビューしてください。変更後は、影響を受ける検証を再実行します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し作業を記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方法を考えるには **Plan**、範囲を絞った変更に継続して関与するには **Interactive**、範囲が明確な作業には **Autopilot** を使います。日常的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに確認する機能

基本的なワークフローは完了しました。Copilot CLI では、次の機能も確認できます。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別の作業を分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。チームで知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、[Foundry Backer Concierge の任意のシリーズ][foundry]に進んでください。別の環境での作業を比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
