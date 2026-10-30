---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、バックログに残った機能要望など、技術的負債を抱えています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプル リクエスト (PR) を作成します。これにより、開発者は時間のかかる作業を任せ、より大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- issue の範囲を明確にする重要性を確認します。
- Copilot に issue を割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した開発者は、ドキュメントをほとんど残しませんでした。まず、アプリケーションのすべてのエクスポート関数に TSDoc doc コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの関連ゲームを表示し、閲覧を続けられるようにします。現時点では、受け入れテストでユーザー体験 (UX) を試せる実装があれば十分です。この作業はブロッカーですが、より優先度の高い issue もあります。

優先順位が下がりやすいこうした作業は、Copilot cloud agent に任せるのに適しています。非同期で作業している間、開発者は別のタスクに集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ると、進捗を記録するドラフトのプル リクエストを作成し、環境を設定してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見簡単な操作にも複雑さが隠れています。[Copilot cloud agent に依頼する作業の範囲][cloud-agent-best-practices]を明確にしましょう。AI ペア プログラマーとして、段階的に作業し、学び、試し、必要に応じて調整してください。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。まだ確認していない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後でその影響を見分けやすくなります。

リポジトリの GitHub Web ユーザー インターフェイス (UI)、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — 特に TypeScript の規約と TSDoc doc コメントに関する **Code standards** を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示が `**/*.test.ts` に適用されることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。プル リクエストに TSDoc doc コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも、これらの点を取り上げます。

## Copilot cloud agent の開発環境を設定する

コードを作成するには通常、環境の準備とセットアップ スクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアと同じように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を用意できます。ラボを円滑に進められるよう、ワークフローはあらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、移行とシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順に必要な範囲で* 権限を最小限に設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリを複製する場合は、`contents: read` 権限が必要です。セットアップ手順で複製しない場合は、手順の完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様の形式ですが、次の点を確認してください。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブから手動で実行できます。Copilot の実行を待たずに、ワークフローをテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、古くなっていたり不足していたりするプロジェクトも少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。ドキュメント作成は Copilot が得意とする作業なので、cloud agent に任せるのに適しています。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエストを開きます。タイトルは `[WIP]: Code lacks documentation` などです。表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した TODO リストを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、作業方法を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状況を把握し、計画を考え、コードを作成する手順を確認できます。

作業には数分かかる場合があります。cloud agent の主な目的は、非同期にタスクを実行し、開発者が別の作業に集中できるようにすることです。この機能を活用するため、別のタスクも cloud agent に割り当て、その間にアプリケーションの機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する間、別の作業に取り組めます。これも cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。作業の完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を割り当てて非同期に作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする方法
- 操作を安全に行う方法
- issue の範囲を明確にする重要性
- Copilot に issue を割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
