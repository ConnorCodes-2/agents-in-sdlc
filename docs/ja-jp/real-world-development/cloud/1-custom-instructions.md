---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に背景情報を与えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング規約、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI の harness と異なり、この演習では変更前と変更後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて比較するのが難しいためです。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データレイヤーには常にユニットテストを用意します。
- UI はダークモードで、モダンな印象にします。
- コードに TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこうした慣行に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令は、コーディングスタイルや要件などのコンテキストを Copilot に与え、より適切な提案やコードを生成できるようにします。リポジトリ全体、または特定のファイルやタスクに対して命令を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に適用されます。技術スタック、開発中の内容、全般的なベストプラクティスなど、ほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語 (TypeScript や Astro など)、UI コンポーネントの作成、ユニットテストの追加など、特定のファイルや作業に関するガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成に使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

このプロジェクトには、代表的な指示ファイルの構成例が用意されています。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構成、全般的なコーディング標準など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなど、ファイル種別や特定タスクのガイドラインを記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成方法は一つではありません。実際に試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトには [UI の指示][ui-instructions]や [Astro の指示][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot を使って指示ファイルを作成することもできます。環境ごとに方法は異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要に応じて各レッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令を確認する

このリポジトリに含まれる `copilot-instructions.md` と複数の `*.instructions.md` ファイルを確認します。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも、Copilot とのあらゆる対話に適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob を指定し、たとえば `**/*.test.ts` に一致する TypeScript テストファイルに指示を適用します。
5. ユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内のファイルへのリンクを確認します。これにより、大きな指示を再利用可能なファイルに分割し、参照例を示せます。ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次に TSDoc コメントとファイル先頭のコメントを追加します。

## リポジトリの標準を追加する

cloud agent が後続の Issue に取り組むとき、新しい標準を使えるように `.github/copilot-instructions.md` を更新します。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. **Code formatting requirements** セクションを探します (27 行目前後)。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントの要件はありません。
4. 既存の標準のすぐ下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリ全体の標準 (`.github/copilot-instructions.md`) や、特定の言語、ファイル種別、タスク向け (`*.instructions.md`) に作成できます。コードコメントに関するプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は、対象の Issue が指定するブランチの指示ファイルを読み込みます。次の演習では cloud agent が `main` からブランチを作成するため、変更を `main` にマージしておきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。以降の演習で cloud agent が指示を使えるようになります。

> [!TIP]
> このワークショップではブランチを使って実際のプロジェクトに近い流れを体験します。直接 `main` で作業する場合は、ブランチを作成せずに `main` にコミットしてもかまいません。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。後続の演習では、cloud agent が作成するプルリクエストを通じて、この標準が使われる様子を確認します。

次は [GitHub Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
