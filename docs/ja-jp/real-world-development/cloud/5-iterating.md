---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に関する複数のタスクを GitHub Copilot と進めてきました。コードにドキュメントを追加し、デザイン チーム向けの関連ゲーム機能を作成し、ハイ コントラストとライト モードを切り替えるアクセシビリティ機能を実装しました。変更を確認し、必要に応じて Copilot にフィードバックを伝えましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人間の監視なしでタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業に使用するブランチ**のみ**に許可されています。
- cloud agent は GitHub Actions 内で、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、想定される動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は作業が終わるまで待ちます。数分かかることがあるため、休憩したり、これまでに確認した内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc コメントなど、更新されたコードとドキュメントを確認します。変更内容は状況によって異なります。演習 2 で確認したカスタム指示ファイルに基づく、TSDoc doc コメント、TypeScript の規約、コメント ヘッダーを探します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot と作業するのは一方通行ではありません。チーム メンバーに依頼するように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は一意ではないため、具体的な文面を指定することはできません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントへの変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をタグ付けします。
2. **View Session** を選択し、Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して構いません。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼できます。たとえば、関連ゲームの表示件数を変えたり、カスタム指示を更新する前に割り当てたタスクであることを踏まえて、コメント ヘッダーや TSDoc doc コメントの追加を依頼したりできます。前と同様に **Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 特に次の点を確認します。
   - モード切り替え用のユーザー インターフェイス (UI) コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード ナビゲーションなど）
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルで次のコマンドを実行し、`<branch-name>` を Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿った実装を Copilot に促したことを確認します。懸念点や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — さらに作業を委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進める方法を試しましょう。リポジトリに issue を追加して Copilot に割り当てます。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリのカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。**
- **Copilot cloud agent に issue を割り当て、環境設定、計画、非同期の作業を確認しました。**
- **アクセシビリティ向けのカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを mission control として使い、進行中のアクセシビリティ セッションを監視して指示を出しました。**
- **cloud agent のプル リクエストをレビューして改善し、`@copilot` に変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き確認する場合は、ほかのハーネスで異なる環境から関連する作業を行えます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[コマンド ライン インターフェイス (CLI) ハーネス](../../cli/)** — Plan モード、Autopilot モード、エージェント スキル、品質保証 (QA) カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
