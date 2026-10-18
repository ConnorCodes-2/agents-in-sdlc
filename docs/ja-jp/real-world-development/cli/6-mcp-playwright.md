---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱ったり、Copilot が利用できる自動化を追加したりする必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれた機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- 未設定の場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには実際に操作する必要があります。Copilot に開発中の Web サイトをユーザーのように操作させ、変更が意図したとおりに動作するかを確かめます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

MCP サーバーは、AI エージェントと外部のツールやサービスをつなぐ役割を果たします。各サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにもさまざまな MCP サーバーがあります。GitHub が提供する [MCP registry][mcp-registry] で、エコシステムの MCP サーバーを探したり、登録したりできます。

> [!CAUTION]
> MCP サーバーも他の依存関係と同じように扱ってください。利用前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Web サイトをユーザーのように操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、この設定手順をスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーを追加するダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が動作することを手動で確認しました。次は Copilot が使える Playwright MCP サーバーで、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP で機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って Issue と計画に照らしてフィルター機能を確認してください。機能を変更せず、動作した点と動作しなかった点を報告してください。確認が終わったら起動したサーバーを停止してください。
   ```

> [!NOTE]
> Copilot は通常、どの MCP サーバーを使えばよいか判断するため、明示的に指定する必要はありません。ただし、使用するツールがわかっている場合はプロンプトに書くことで、結果を安定させ、トークンを節約できます。

2. Copilot がブラウザーで確認する手順を実行する様子を見ます。
3. 報告内容を読み、機能が期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。

- Model Context Protocol と Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーが未設定の場合に追加しました。
- エージェントにブラウザーを操作させ、機能を確認しました。

次はスキルとブラウザーツールを組み合わせた[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
