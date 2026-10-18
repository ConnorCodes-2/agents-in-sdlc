---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このワークショップでは、ユーザー体験の改善と機能追加に関するいくつかのタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードを含むアクセシビリティ機能の実装を依頼しました。変更を確認し、必要であれば Copilot にフィードバックを返しましょう。

### シナリオ

生成 AI を使っても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ人間の監視なしにタスクを実行します。安全性を保つため、次の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コード作業用のブランチに限って書き込みできます。
- Cloud agent は GitHub Actions 内で、個別の一時的な環境を作成して実行されます。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- [外部リソース][agent-firewall]へのアクセスは、MCP サーバーを含め、既定で制限されています。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成したコードドキュメントの PR を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへのアクセスを含め、既定で外部リソースへのアクセスが制限されているため、これは想定された動作です。必要に応じて、[cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選び、PR の一覧を開きます。
3. **Add missing documentation** のようなタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかる場合があるので、学んだことを振り返りながら待ってください。

4. **Files changed** タブを選び、変更を確認します。
5. 追加された TSDoc コメントなどのドキュメントを確認します。変更内容は実行結果によって異なります。演習 2 で確認したカスタム指示に由来する TSDoc コメント、TypeScript の規約、コメントヘッダーにも注目します。
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちであることを示す表示がないか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。
9. PR のチェック欄にワークフローが登録されます。正常に進めば、Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot とやり取りする方法は、変更を一方的に受け取るだけではありません。チームメンバーと同じように、PR のコメントやコードのインラインコメントで Copilot に言及できます。Copilot はコメントを確認し、追加のセッションを開始して対応します。出力は確率的であるため、どの依頼を送るべきかは一つに決められません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、ドキュメントの変更を依頼するコメントを追加し、**@copilot** に言及します。
2. **View Session** を選び、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選ぶと PR に戻ります。
4. 作業が終わると、PR に新しいコミットが追加されています。
5. **Files changed** タブを選び、変更を確認します。

納得できるまで反復して改善してください。準備ができたらドラフト PR をレビュー可能な状態にし、`main` にマージできます。

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue の PR を確認します。

1. GitHub.com のリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルの PR を選択します。
4. **Files changed** タブで、生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローが承認待ちであることを示す表示がないか確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。
8. PR のチェック欄でワークフローの実行状況を確認します。Astro アプリのチェックが成功するまで数分かかる場合があります。
9. **任意:** Codespace でブランチを開き、関連ゲーム機能を手動でテストできます。ターミナルで、Copilot が作成したブランチ名 (例: `copilot/fix-8`) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。ほかの PR と同じように、会話タブにコメントを追加して、表示する関連ゲーム数やコメントヘッダー、TSDoc コメントなどの変更を反復して依頼できます。この Issue を割り当てた時点では、カスタム命令を更新する前だったことに注意してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から依頼したライトモードが含まれているはずです。

1. GitHub.com のリポジトリを開きます。
2. **Pull Requests** タブを選びます。
3. **Add high contrast mode to website** のようなタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が表示されます。数分待ってください。

4. **Files changed** タブで生成されたコードを確認します。
5. 次の点に注目します。
   - 表示モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使い方。
   - ハイコントラストモードとライトモードの CSS やスタイル。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モードを切り替える JavaScript/TypeScript コード。
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちであることを示す表示がないか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。
9. PR のチェック欄でワークフローの実行状況を確認します。Astro アプリのチェックが成功するまで数分かかる場合があります。
10. **任意:** Copilot が作成したブランチを Codespace で開き、アクセシビリティ機能を手動でテストできます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、ベストプラクティスに従った機能実装を Copilot に促したことがわかります。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に修正を依頼できます。

## 任意の演習 — 作業を委任し続ける

cloud agent は、実際のバックログ項目を任せている間に別の作業を進めると役立ちます。リポジトリに Issue を追加し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。

- リポジトリに含まれるカスタム命令を確認し、cloud agent の結果に与える影響を学びました。
- Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードを追加しました。
- Copilot agents ページを mission control として使い、セッションを監視して途中で指示しました。
- cloud agent の PR をレビューして改善し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent harness はここまでです。ほかの環境のワークショップで、今回学んだ内容をさらに広げられます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP の連携を試します。
- 💻 **[CLI harness](../../cli/)** — Plan と Autopilot モード、agent skills、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への `/delegate` による任意の委任を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでも、次のアイデアを試してみてください。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

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
