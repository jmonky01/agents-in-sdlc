---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を決め、Autopilot で issue からフィルタリングを実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから、issue に基づく機能の作成、コードの検証、動作確認、プロジェクトへのマージという、エージェント駆動の開発プロセスを進めます。

> [!NOTE]
> 機能の作業では、同じ会話とブランチを使います。通常はファイル種別ごとにブランチやプル リクエスト (PR) を分けますが、このワークショップでは主要な概念に集中するため、手順を簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を開始します。
- Plan mode で要件を決めます。
- Autopilot mode で新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加し、品質保証 (QA) agent を作成して機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えており、訪問者はカテゴリと publisher でゲームを絞り込みたいと考えています。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に決めるべき詳細があります。Plan mode で方針を決めてから、Autopilot で範囲を限定した実装を行います。

## 背景

AI コーディング エージェントを使っても、開発の基本は変わりません。一般的な開発フローは次のとおりです。

1. 作業内容を説明する issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の流れに沿って進めます。

標準的な開発フローに沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同様の検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、次の作業に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan mode で計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリング作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新してフィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトは `#7` などで始まります。`#` を使って GitHub の issue やプル リクエスト (PR) を会話のコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に作業を依頼する前に機能の実装方針を決められます。複雑なタスクでは、事前に計画することをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な実装方法を検討して計画を作成します。

3. フィルタリングの動作について質問されたら、自分の希望に基づいて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読みます。
6. 自分の判断と一致しない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot で実装します。Autopilot はタスクが完了したと判断するまで、Copilot に作業を反復させます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選択します。
2. モード表示に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、実装がすぐに始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了とみなす前に環境の問題を解決します。

## 実装をレビューして検証する

生成コードは、ほかのコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示に **Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. issue と計画時に決めた内容に照らして変更を確認します。
4. コードのレビュー後、<kbd>Esc</kbd> を押して diff 表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を確認する

コードを確認したら、アプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot に次のプロンプトを送り、アプリを起動してもらいます。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリと publisher のフィルタリング、および組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。

- GitHub issue から Copilot の会話を開始しました。
- Plan mode で要件を決めました。
- Autopilot mode で新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された開発プラクティスに沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
