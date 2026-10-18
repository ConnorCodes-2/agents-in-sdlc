---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同じように、GitHub Copilot CLI には操作用のスラッシュコマンドが多数あります。高度な機能や内部の状態、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で会話のコンテキストを確認します。
- `/model` で利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使うコマンドを確認します。

## シナリオ

CLI の主要なワークフローが終わりました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大コンテキストウィンドウに近づく場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュコマンドを使って自分で確認、圧縮することもできます。

1. Codespace に戻り、Copilot CLI がまだ起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域ごとのコンテキスト量を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比べます。会話がすでに短い場合は、大きく変化しないことがあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選ぶ場合は `/compact` を使います。関係のない新しいタスクに移る場合は `/clear` または `/new` で新しい会話を始めます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist に公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、`/share file` のようなローカル Markdown エクスポートなど、目的に合った保存先を選びます。
3. エクスポート内容を確認してから、他の人に送信したり公開したりします。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を並列化する、または委任する

ワークショップの主要な内容はここまでです。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分けて並列実行できます。
- `/worktree` は別タスク用の分離された Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信できます。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドは worktree の作成やリモート作業を開始する場合があるため、任意機能です。試すときは、新しい会話で範囲を明確にしたタスクを開始し、通常のワークフローで結果をレビューしてください。非同期のエージェント作業について詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

スラッシュコマンドを使って Copilot CLI を構成し、内部の動作やセッションを確認しました。

- `/context` と `/compact` で会話のコンテキストを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポート、共有する方法を学びました。
- 並列作業、worktree、cloud agent への任意の委任に使うコマンドを確認しました。

ほかにも多くのスラッシュコマンドがあります。最後に[学んだ内容と今後の学習方法を振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
