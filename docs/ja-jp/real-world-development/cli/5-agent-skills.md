---
title: "レッスン 5 - quality-checks スキルを調整して使う"
description: "既存の quality-checks スキルを確認し、レポート形式を調整して、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手作業で動作を確認し、指示ファイルで標準に従わせることはできました。では、テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には **agent skill** が適しています。スキルは、テストなどの操作を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式を調整する。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスク指示、実行可能なスクリプト、サポート リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーに `SKILL.md` という Markdown ファイルを置きます。このファイルには、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、スクリプトやほかのリソース用のサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有、再利用するためにプロジェクトの `.github/skills` フォルダーに置くか、ユーザーのスキル フォルダー `~/.copilot/skills` に置くことができます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックのために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。説明によって Copilot がスキルを呼び出すタイミングを判断できることを確認します。
3. 指示を読み、テストと lint の流れをどのように導いているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接、または自然言語で呼び出せます。Copilot に 3 つのチェックを実行してもらいます。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートを調整する

実行した内容、成功したかどうか、ツールから実際に報告された内容が分かる、よりよいレポートにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルは、会話を再起動せずに Copilot CLI で再読み込みできます。

1. 次を入力します。

   ```text
   /skills reload
   ```

2. 先ほどと**まったく同じ**プロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill を調整して使いました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果のレポート形式を調整した。
- スキルを再読み込みして実行した。

この変更はフィルター機能の PR に含まれます。次は [Playwright MCP サーバー][next-lesson]を使い、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
