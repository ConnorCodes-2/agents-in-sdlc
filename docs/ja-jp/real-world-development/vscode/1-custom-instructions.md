---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を知らせたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関するガイダンスを与えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示でフィルター機能の最初のデータスライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再度実行し、新しい標準がコードに反映されることを確認します。
- 次の演習で利用できるよう、指示の変更とフィルターの基礎を `main` にコミットします。

> [!CAUTION]
> 生成コードが標準の一部に従わない場合があります。Copilot の動作は確率的です。この演習では、出力を文字単位で同じにするのではなく、指示を更新した後にコード生成の傾向がどう変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを追加します。
- UI はダークモードで、モダンな印象にします。
- コードに TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこうした慣行に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令は、コーディングスタイルや要件などのコンテキストを Copilot に与えます。リポジトリ全体、または特定のファイル種別のために命令を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**要求に送信されます。技術スタック、開発中の内容、全般的なガイダンスなど、プロジェクトに関する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネント作成、ユニットテスト作成などの特定の作業にガイダンスを適用します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成に使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

- `copilot-instructions.md` には、プロジェクトの説明、構成、全般的な標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や特定タスクの指示を記載します。
- 自然な言葉で明確に説明し、コードの例と反例を示します。

指示ファイルの作成方法に唯一の正解はありません。試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] などの指示ファイルがあります。Copilot に指示ファイルを作成させることもできます。環境ごとに方法が異なり、たとえば VS Code では **Configure Chat → Generate Agent Instructions** を、Copilot CLI では `/init` を使います。テンプレートを探す場合は [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などの項目を確認します。**Code standards** の **GitHub Actions Workflows** のガイダンスも Copilot とのやり取りに適用されます。
3. `.github/instructions` を開き、Astro、Drizzle、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` の glob が `**/*.test.ts` などの TypeScript テストファイルに適用されることを確認します。
5. ユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示やプロジェクト内のファイルへの相対リンクを確認します。大きな指示を再利用可能なファイルに分けられます。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントの要件はありません。次に TSDoc コメントとファイルコメントの要件を追加します。

## 変更用のブランチを作る

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選びます。モードとモデルは演習ごとに異なるため、各手順で説明します。

指示の影響を見るため、まず現在の指示ファイルの状態でプロンプトを送信します。後で指示を更新して同じプロンプトを再送信します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使えるようにします。
2. `src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント一覧から **Agent** を選び、ファイルを変更できるようにします。
4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、すべての発行元について name と id を返すデータアクセスヘルパーを作成または更新してください。ファイルを変更してください。
   ```

5. Copilot がプロジェクトを調べ、ヘルパーとテストなどのコードを更新します。
6. 提案されたヘルパーが `db` クライアントを第 1 引数に受け取り、発行元の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約によるものです。
7. TSDoc コメントとファイルヘッダーが**ない**ことに注目します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が改善されることが、この演習の目的です。

## リポジトリの標準を追加する

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コード内ドキュメントのルールがまだないことを確認します。
3. 既存の標準のすぐ下に次の内容を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. ファイルを保存します。

> [!TIP]
> リポジトリ全体の標準は `.github/copilot-instructions.md` に、特定の言語、ファイル種別、タスクの指示は `*.instructions.md` に記載します。ドキュメントコメントのルールはリポジトリ全体に適用するため、リポジトリレベルのファイルに追加します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話のコンテキストをクリアします。
2. `src/lib/publishers.ts` を開き、Copilot の対象ファイルにします。
3. 前と同じプロンプトを送ります。

   ```plaintext
   src/lib/publishers.ts に、すべての発行元について name と id を返すデータアクセスヘルパーを作成または更新してください。ファイルを変更してください。
   ```

4. 提案ファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームの発行元データアクセスヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * id と name を含むすべての発行元の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使用する実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 変更内容を次のコマンドで確認します。

   ```bash
   git status
   ```

3. 指示ファイルと Copilot がヘルパー用に変更したファイルをステージしてコミットします。追加のテストなどがある場合は、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** を選択します。
3. 次のプロンプトで、指示に関する Issue を探し、現在のブランチから PR を作成するよう依頼します。

   ```text
   指示ファイルの更新に関する Issue を探してください。現在のブランチから PR を作成し、その Issue をクローズすることと、後のフィルター作業に備えて発行元ヘルパーの基礎も追加したことを説明してください。
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub 上の操作を許可するよう求められたら、内容を確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻して最新の状態にします。

   ```text
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull してください。
   ```

7. GitHub 上の操作やシェルコマンドの許可を求められたら、内容を確認して適切に許可します。

GitHub Copilot の支援で PR を作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示を利用する仕組みを確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示でコードを生成し、結果を確認しました。
- `.github/copilot-instructions.md` に標準を追加しました。
- 同じプロンプトを再実行し、新しい標準が提案に反映されることを確認しました。
- 指示の変更と発行元ヘルパーの基礎を `main` にコミットしました。

次は[エージェントモード][next-lesson]で指示を利用し、Copilot にコードベース全体の機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタム命令][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
