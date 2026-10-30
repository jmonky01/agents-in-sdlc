---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使って、コード生成に関するガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- 現在の指示を使ってフィルタリング用のデータ層の最初の部分（publisher ヘルパー）を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリングの基礎を `main` にコミットし、Copilot が次の演習で新しい指示を使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、出力の一字一句を一致させるのではなく、指示を更新した後の動作の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。データ層には必ずユニット テストを用意すること、ユーザー インターフェイス (UI) をダーク モードにして現代的な印象にすること、TSDoc doc コメントでコードを文書化すること、各ファイルの先頭に役割を説明するコメント ブロックを追加することなどです。指示ファイルを使い、これらの開発手法に沿ってタスクを実行できるよう Copilot に情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝え、より適切な提案やコード スニペットを得やすくなります。推奨するコーディング規則、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体に適用する指示も、特定のファイル タイプやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル タイプ向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの作成などに関するガイドラインを指定できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

サンプル プロジェクトの例は、指示ファイルを作成する一般的な方法を示しています。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体的なコーディング標準などのプロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ層などのファイル タイプ、または特定のタスク向けの指示を記載します。
- 自然な言葉で明確に記述し、コードの良い例と避けるべき例を示します。

指示ファイルの作成方法も AI の使い方と同じく、決まった方法が一つだけあるわけではありません。プロジェクトに合う方法は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot に指示ファイルの作成を手伝ってもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、方法は環境によって異なります。関連する手順は該当するレッスンで説明します。
>
> テンプレートを探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる基本の `copilot-instructions.md` と、タスク向けの `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドがリポジトリ ルートからの glob を指定します。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルなどが対象です。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールし、`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、コード生成の例を Copilot に示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。後の手順で TSDoc doc コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

作業用のブランチを作成してから変更します。

1. 前の演習の Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

現在の指示ファイルを使ってプロンプトを送り、カスタム指示の効果を確認します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの場所を Copilot に示します。
3. Chat ビューのエージェント選択から **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューの agent 選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、必要なコードを作成します。ヘルパーとテストなど、複数のファイルが更新される場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づきます。
7. 提案されたコードに TSDoc doc コメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する場合があります。その場合も問題ありません。この演習の目的は、指示更新後の一貫性の変化を確認することです。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えます。コーディング標準を記載して、コード提案の改善につなげましょう。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメント作成ルールはありません。
3. 既存の標準の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ レベルの指示ファイルは全体に適用し、`*.instructions.md` ファイルは特定の言語、ファイル タイプ、タスクに適用できます。今回の doc コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

同じプロンプトを使い、更新した指示がコード提案に反映されるか確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を開き、Copilot が対象ファイルに注目できるようにします。
3. 前と同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。指示の更新とヘルパーの基礎をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

プッシュしたブランチから PR を作成し、バックログにあるドキュメント標準の issue に関連付けます。GitHub ツールがプロジェクトに接続されているため、Copilot に issue を検索して PR を作成し、マージするよう依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールが設定済みです。初回に VS Code からサインインを求められた場合は、表示される手順に従って許可してください。後の演習でこの接続（Model Context Protocol）について説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 次のプロンプトを使い、指示ファイル更新の issue を探して、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成するまで待ちます。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して必要な操作を許可します。
6. PR 作成後、次のプロンプトを使って PR をマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの **Allow** を求められた場合は、コマンドを確認して必要な操作を許可します。

GitHub Copilot の支援で PR を作成し、マージしました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は [agent mode][next-lesson] を使って、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
