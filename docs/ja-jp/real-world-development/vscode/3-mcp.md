---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを作成する前に、ブラウザーで動作することを確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを提供する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーからフィルター機能をテストしてもらいます。
- ブランチを公開し、フィルター作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にあるツールも使えるとさらに便利になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードがコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと連携する様子を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わる操作を実行できます。

MCP サーバーは、エージェントと外部ツールをつなぎます。GitHub API や NPM パッケージのようなローカルツールと通信し、エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: リポジトリ、Issue、プルリクエストを管理する GitHub API を利用できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を利用できます。Web ページを開いたり、フォームに入力したり、ボタンを操作したりできます。

ほかにも多くの MCP サーバーがあります。GitHub の [MCP registry](https://github.com/mcp) で見つけたり、エコシステムに貢献したりできます。

> [!CAUTION]
> MCP サーバーもプロジェクトの依存関係と同じように扱ってください。利用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密情報や操作へのアクセス権を付与するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに次の `github` サーバーが設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` 設定はプロジェクトテンプレートに含まれているため、前の演習で Copilot は Issue のバックログを読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` と並ぶ位置に `playwright` を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` オプションは画面を表示せずにブラウザーを実行します。デスクトップ画面がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストにすでに Playwright を使っているため、必要なブラウザーは通常インストール済みです。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行してもう一度試します。

## サーバーを起動して信頼する

VS Code は必要になったときに MCP サーバーを起動します。初めてサーバーのツールが必要になると、VS Code が起動し、信頼するかどうか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に表示される **Start** を選び、サーバーを今すぐ起動します。これにより設定が正しいことを確認できます。
2. VS Code がサーバーを信頼するか尋ねたら、設定を確認して信頼することを選びます。

手動起動は省略できます。その場合は次の手順で Copilot が初めてツールを使うときに起動され、信頼を求めるメッセージが表示されます。

## フィルター機能をテストする

Playwright MCP サーバーを使うと、エージェントが実際のブラウザーを操作してページを開き、フィルターを適用し、結果を確認して報告できます。エージェントはスクリーンショットではなく、[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を使って推論するため、機能の確認が簡単なアクセシビリティチェックにもなります。

サーバーに接続してアプリを起動したら、次のプロンプトを Copilot に送ります。

```text
Playwright MCP サーバーを使って、実行中の http://localhost:4321 のアプリをブラウザーで開き、新しいゲームのフィルター機能を確認してください。

1. ゲーム一覧ページを開き、ゲームの件数を記録します。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけに一覧が更新されることを確認します。
3. フィルターを解除して、発行元フィルターを適用し、その発行元のゲームだけに一覧が更新されることを確認します。
4. カテゴリと発行元のフィルターを組み合わせ、両方の条件に沿った結果が表示されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP を使ってブラウザーを起動し、手順どおりに操作して結果を報告します。Issue の受け入れ条件と報告内容を比べてください。問題があれば追加で質問するか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> テストには `http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要なことがあります。不足が報告された場合は `npx playwright install chromium` を実行してもう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot の変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択し、コミットメッセージを生成します。

    ![Copilot の変更を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub CLI などから PR を作成できます。ここでは Copilot に関連する Issue を探して、PR を作成してもらいます。

1. Copilot Chat パネルを開き、**New Chat** を選択します。
2. GitHub ツールを使えるように、エージェントの一覧から **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリと発行元によるフィルターに関係する Issue を探してください。現在のブランチからプルリクエストを作成し、正しい Issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、情報収集や操作を許可します。GitHub ツールを初めて使う場合は認証を求められることがあります。画面の指示に従ってください。
5. Copilot が Issue を検索して PR を作成する様子を確認します。
6. Copilot が表示するリンクから PR を開いてレビューします。まだマージしないでください。

## まとめと次のステップ

- MCP と、MCP サーバーによる Copilot の拡張方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- PR を作成する前に、ブラウザーから機能をテストしました。
- ブランチを公開し、フィルター機能の PR を作成しました。

MCP サーバーで機能をテストしました。Copilot を拡張する方法はほかにもあります。次は、アクセシビリティレビューなどのタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
