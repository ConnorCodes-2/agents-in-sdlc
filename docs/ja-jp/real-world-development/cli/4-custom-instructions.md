---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合は、Copilot が参照できるようにガイダンスを用意します。[指示ファイル][instruction-files]を使うと、「どのような」コードが必要かだけでなく、「どのように」構成するかも伝えられます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示ファイルとパス別の指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、引数、戻り値を説明する TSDoc/JSDoc を付けます。該当する場合は注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこうした慣行に沿って作業できるようにします。

## 指示ファイル

カスタム命令は、コーディングスタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体、または特定のファイルやタスクに対して命令を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に適用されます。ほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクのガイドラインを記載します。

> [!NOTE]
> その他の指示形式と環境ごとの対応状況は異なります。利用する形式を決める前に、[カスタム命令の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、指示ファイルがすでに含まれています。変更を加える前に確認し、影響を比べられるようにします。

1. Codespace に戻ります。
2. ターミナルではなく Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分けたり、Copilot に例を示したりできます。

## チームのガイダンスに合わせて指示を更新する

新しい TypeScript コードに TSDoc コメントが追加されるよう、`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します (35 行目前後)。
2. 最後の項目として、次の Markdown を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話を開始するときにリポジトリの指示を読み込みます。指示を更新した後、フィルター機能の会話を終了して再開すると、機能のコンテキストを保ったまま新しい指示が読み込まれます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動保存します。
2. `game-filters-cli` ブランチのリポジトリのルートで、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のプロンプトを送ります。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードを新しいガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力して TypeScript ファイルの変更を確認します。新しい TSDoc コメントが追加され、内容を正確に説明していることを確認します。

## まとめと次のステップ

指示ファイルから Copilot にコンテキストが伝わる仕組みを確認し、新しい標準を機能に適用しました。

- リポジトリとパス別の指示ファイルを確認しました。
- `.github/copilot-instructions.md` に標準を追加しました。
- その標準がフィルター機能のコードに適用されることを確認しました。

次は [再利用可能な quality-checks スキルを調整して実行し][next-lesson]、lint とテストを一貫して行えるようにします。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令の対応状況][custom-instructions-support]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
