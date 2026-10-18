---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行して、外部ツールと連携できます。すべてコマンド ラインから行えるため、作業に集中したままタスクを任せたり、変更を依頼したりできます。まずはツールをインストールしましょう。すでに使い慣れているツールからインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーでフィルター Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルにその機能をもたらします。このレッスンでインストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイス コードの手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認してから、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェル プロンプトでは Codespace でコマンドが実行されます。Copilot CLI の起動後は、自然言語の入力がエージェントに送られ、スラッシュ コマンドで会話を操作します。

1. `/model` と入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押してから、もう一度 <kbd>Enter</kbd> で確定します。
2. `/help` と入力して、インストールしたバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の簡単な質問をして、正常に動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答の前にリポジトリを調べる様子を確認します。
5. `/mcp list` と入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のように依頼し、フィルター Issue を見つけます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更を終えた後にこの Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree を作成することはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可されていないツールを使う前に確認を求めます。このワークショップでは `--yolo` を指定して再起動し、Codespace 内の承認プロンプトを省略して演習に集中します。

> [!CAUTION]
> `--yolo` はすべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使用してください。日常的な開発の既定値として alias に設定しないでください。詳しくは[ツールの使用許可と拒否][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後続のレッスンで Issue やプルリクエストを操作するための GitHub MCP 読み取り / 書き込みツールが有効になります。Codespace によりローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` で同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーでフィルター Issue を見つけた。

次は、ゲーム カードに星評価を表示する最初の変更に取り組みます。[最初の小さな変更を始める][next-lesson]に進んでください。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
