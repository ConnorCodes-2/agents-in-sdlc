---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織が技術的負債を抱えています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がないままバックログに残っている機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せ、開発者がより大きな課題に集中できるようにします。

この演習では、次のことを学びます。

- コード生成環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- 範囲を明確に定めた Issue の重要性を学びます。
- Copilot に Issue を割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初にサイトを作った開発者はドキュメントを残しておらず、まずアプリケーション内のエクスポート関数すべてに TSDoc コメントを追加したいと考えています。

また、ゲームの詳細ページに同じカテゴリの関連ゲームを表示し、ユーザーがゲームを見つけやすくしたいとデザインチームが考えています。完成度の高い実装でなくても、UX の受け入れテストに使える状態を求めています。現在はブロッカーですが、より優先度の高い Issue がほかにあります。

どちらも優先度が下がりがちな作業です。Copilot cloud agent に非同期で進めてもらい、開発者は別の作業を行ってから結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot はドラフトプルリクエストを作成して進捗を記録し、環境を準備して作業を始めます。進行中または完了後にセッションを確認できます。解決案のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような機能はありません。考える必要がなくなるわけではありません。単純に見える作業にも複雑さがあります。[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、適応します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令が Copilot を導く仕組みを学びました。`.github/copilot-instructions.md` と `.github/instructions/unit-tests.instructions.md` を読み取り専用で確認し、cloud agent に作業を割り当てる前にどのような影響があるか見ておきましょう。

- `.github/copilot-instructions.md` の **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する標準を確認してください。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` frontmatter を確認します。`**/*.test.ts` ファイルにこの指示が適用されます。

次の **Code lacks documentation** Issue を cloud agent に割り当てる際に、TSDoc コメント、TypeScript の規約、ファイルヘッダーがプルリクエストに反映されるか確認します。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を準備する

Copilot にタスクを割り当てる場合も、一般的なソフトウェア開発と同様に、コードの作成に必要な環境とセットアップスクリプトが必要です。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に [セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。演習のため、テンプレートには次の環境が事前設定されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザー、Astro アプリ用にマイグレーションとシードを行った SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできるようにします
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には Copilot 専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。手順でクローンしない場合、完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースのマイグレーションとシードを行います。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` により、リポジトリの **Actions** タブから手動で実行できます。Copilot の作業を待たず、ワークフローが成功するかをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、古いままだったり、そもそもなかったりするプロジェクトも少なくありません。こうした技術的負債は見過ごされやすく、作業効率を落とし、新しい開発者がチームに加わりにくくします。Copilot はドキュメント作成を得意とするため、cloud agent に依頼するのに適した Issue です。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、説明が有用な関数とメソッドに TSDoc コメントを付けることが求められています。最近の更新でこの標準が守られていないため、動作を明確にする既存コードにドキュメントコメントを追加してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択します。
9. **Assign** を選択します。
10. **Pull Requests** タブを開きます。
11. `[WIP]: Code lacks documentation` のようなタイトルの PR を開きます。表示されない場合は、少し待ってページを更新します。
12. Copilot が作成した TODO リストを確認します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかる場合があります。Copilot が先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題への対処方法を決めているためです。

13. Copilot が実行するタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを確認します。
15. **View session** を選択します。
16. セッションを確認します。Copilot はコードと状態を調べ、計画を考えてからコードを作成します。

完了まで数分かかることがあります。cloud agent が非同期にタスクを実行する間に、別の作業を進められます。もう 1 つタスクを割り当てたら、アプリケーションの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent の利点は、作業を分けて並行できることです。デザインチーム向けの関連ゲーム機能を cloud agent に依頼します。

1. github.com のリポジトリに戻ります。
2. **Issues**、続いて **New issue** を選択します。
3. **Blank issue** を選びます。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次の要件を入力します。

    ```markdown
    ゲームの詳細ページに関連ゲームを表示し、ゲームを探しやすくしたいと考えています。要件は次のとおりです。

    - `src/lib/` に、指定したゲームと同じカテゴリに属する別のゲームを返すデータアクセスヘルパーを追加します。指定したゲーム自体は除外します。
    - ゲーム詳細ページにヘルパーを使う「Related games」セクションを表示します。
    - 関連ゲームがない場合にも対応します。
    - 新しいヘルパーのユニットテストを作成します。
    - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

6. **Create** を選択して Issue を作成します。
7. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。
8. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

Copilot は 2 つ目の依頼にも取り組み始めました。人間の開発者と同様に動作するため、常に監視する必要はありません。完了したらレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使うと、Issue を Copilot に割り当て、非同期にタスクを実行させることができます。技術的負債への対応、新機能の追加、フレームワーク間のコード移行などに活用できます。

- コード生成環境をカスタマイズしました。
- 安全に操作することの重要性を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Copilot に Issue を割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[cloud agent では MCP サーバーも使えます][cloud-agent-mcp]。前のモジュールで学んだカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
