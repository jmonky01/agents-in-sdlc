---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に依頼する内容だけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code やコマンド ライン インターフェイス (CLI) のハーネスとは異なり、この演習では変更前後のプロンプトを使いません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューすると、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。次の内容が含まれます。

- データ レイヤーには、必ずユニット テストを用意します。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な印象にします。
- TSDoc doc コメントを使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、これらの開発手法に沿ってタスクを実行できるよう Copilot に必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝えられます。これにより、より適切な提案やコード スニペットを得やすくなります。推奨するコーディング規則やライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル タイプやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送信するチャットや CLI のリクエストの大半に関係する、プロジェクト レベルの情報を記述します。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル タイプ向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、新しいユニット テストの作成などのタスクに関するガイドラインを指定できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトの例は、一般的な方法を示しています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル タイプ、または特定のタスク向けの指示を記載します。
- 自然な言葉で、明確なガイダンスを記載します。コードの良い例と避けるべき例も示します。

指示ファイルの作成方法も AI の使い方と同じく、決まった方法が一つだけあるわけではありません。プロジェクトに合う方法は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルの作成を手伝ってもらうこともできます。方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。該当するレッスンで、関連する場合に説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認します。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決める glob（リポジトリ ルートからの相対パス）を指定していることを確認します。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、コード生成の例を Copilot に示す方法がわかります。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc doc コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内のドキュメントに関するプロジェクト全体のルールを追加することです。TSDoc doc コメントとファイル レベルのコメント ヘッダーを必須にします。cloud agent は、このハーネスの後半で割り当てる issue に取り組むときに、このルールを参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、変更を整理してコミットするためにブランチを使います。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前のセクションで説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を記載し、コード提案の改善につなげましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準を記載していますが、生成されたヘルパーに doc コメントがなかった理由となる、コード内のドキュメント作成に関するルールがまだありません。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイル コメント ヘッダーと TSDoc doc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に作成して全体に適用することも、特定の言語、ファイル タイプ、タスク向けの `*.instructions.md` として作成することもできます。今回追加した doc コメントのルールのような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチから指示ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` に反映しておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で割り当てる issue に取り組む際、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略し、`main` に直接コミットできます。ここでは実際のプロジェクトでの作業方法に近づけるためにブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で cloud agent が作成するプル リクエストをレビューすると、追加した標準が適用されていることを確認できます。

次に、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
