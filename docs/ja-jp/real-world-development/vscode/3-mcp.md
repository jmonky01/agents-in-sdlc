---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを手動で操作する代わりに、**Playwright Model Context Protocol (MCP) サーバー**を接続し、Copilot に実際のブラウザーでテストしてもらいます。その後、ブランチを公開してプル リクエスト (PR) を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストしてもらいます。
- ブランチを公開して、フィルタリング作業の PR を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部に接続できるとさらに便利になります。Model Context Protocol (MCP) は、Copilot が外部ツールやサービスと通信するための標準です。

![agent mode とコンテキスト、大規模言語モデル (LLM)、ツール、および MCP サーバーと VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP によって、リソースを使って最新情報にアクセスしたり、ツールを使って代わりに操作を実行したりできます。

MCP サーバーは、AI エージェントと外部ツールやサービスの間をつなぎます。既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが使えるツールとリソースのセットを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供し、リポジトリの作成や更新、issue とプル リクエストの管理などを可能にします。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供し、Web ページの移動、フォーム入力、ボタン選択などを可能にします。

ほかにもさまざまな MCP サーバーがあります。GitHub は、見つけやすさとエコシステムへの貢献を促進する [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードと発行元を確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使える MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot は issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、初回利用時に GitHub 認証を行います。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能をテストできます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行する指定です。デスクトップ画面がない Codespace ではこれが必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使うため、通常はブラウザーがすでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してから再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うときに、VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択し、サーバーを起動して設定を確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して信頼することを選択します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot はブラウザーを操作できます。ページを開き、移動し、フィルターを適用して、結果を確認できます。会話を離れずに機能を検証できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を扱うため、機能の確認とともに簡易的なアクセシビリティ確認もできます。

サーバーを接続してアプリを起動した状態で、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らして確認します。問題があれば、追加で質問するか、PR を開く前にコードを修正するよう依頼します。

> [!NOTE]
> テストにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーの初回利用時にブラウザーがダウンロードされる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認したので、チームのレビュー用に PR を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が作成した変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com やコマンド ライン インターフェイス (CLI) など、複数の方法で PR を作成できます。ここでは GitHub Copilot に作成してもらいます。関連 issue を検索し、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント選択で **Agent** が選択されていることを確認します。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報収集や操作を実行できるようにします。GitHub ツールの初回利用時に認証を求められた場合は、表示される手順に従います。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開して、フィルタリング作業の PR を作成しました。

機能のテストに MCP サーバーを使いましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタム エージェント][next-lesson]を作成し、アクセシビリティ レビューなどの作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
