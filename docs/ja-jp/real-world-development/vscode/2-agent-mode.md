---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリ更新でも、複数のファイル変更やテスト実行が必要になることがあります。必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、変更漏れがあればさらに修正する作業を繰り返します。

Copilot Agent Mode を使うと、統合開発環境 (IDE) 内でより自律的に作業できます。既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を修正します。Agent Mode を使って、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は用途が異なります。agent mode は IDE 内で動作し、すばやくフィードバックを得ながら対話できます。cloud agent はチームの一員のように非同期で作業し、issue とプル リクエストを通じてやり取りします。

この演習では、次のことを行います。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を行う方法を確認します。
- UI とデータ層にまたがる新機能を実装します。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューします。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリと publisher で絞り込めるようにします。前の演習で追加した publisher ヘルパーを使い、Copilot Agent Mode で残りのデータ層、ユーザー インターフェイス (UI)、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内から Drizzle ORM を使ってローカルの SQLite データベースを直接照会します。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準ドキュメント オブジェクト モデル (DOM) API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Codespace で `npm run dev` を実行すると、単一の Astro 開発サーバーが起動します。

1. Codespace に戻ります。前の演習後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーの `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、現在の機能を確認します。

- **Home Page**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されています。GitHub Copilot にバックログを表示するよう依頼します。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。

1. Chat ビューのエージェント選択から **Agent** を選択します。Agent は複数ファイルにわたる変更を計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent 選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。大規模言語モデル (LLM) は確率的に動作するため、提案が記載内容と異なる場合があります。これは通常の動作です。

> [!NOTE]
> 大規模言語モデル (LLM) の確率的な性質により、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを送信し、issue のバックログについて Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コード生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。[ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する際の指針があります。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計、テスト容易性、アクセシビリティの原則、ファイル タイプ別の指示へのリンクが含まれます。
   - Astro
   - Tailwind CSS
   - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用したり、複雑な内容を小さく分けたり、例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、データ層 (`src/lib/`) のロジック、動作を確認するテスト、ゲーム一覧ページの UI を更新する必要があります。マージ前にテストも実行して成功させます。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent 選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層、UI、テストを特定して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調査しているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行うため、タスクやコードベースによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。作業完了後にすべての変更を確認できるため、進行中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで作業を続けます。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。

- UI とデータ層にまたがる新機能を実装しました。
- プロジェクトを調査し、関連ファイルを特定して、複数ファイルにわたる変更を調整する方法を確認しました。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだ方法を使い、Copilot にページ分割を追加するよう依頼できます。既存の指示ファイルを使い、ページ サイズをユーザーが選択するか固定値にするかを検討し、必要なコンテキストをプロンプトに含めます。必要に応じて Copilot に変更と追加のコンテキストを伝え、反復して改善してください。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
