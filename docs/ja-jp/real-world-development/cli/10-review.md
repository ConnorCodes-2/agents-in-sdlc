---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、次の学習リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローに取り組みました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを調べて、フィルター機能の Issue を確認しました。
- 星評価を追加して、転送ブラウザーで動作を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から作業を始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルを調整して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加して、実際のブラウザーでフィルター機能を確認しました。
- QA カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーの検証結果を確認しました。
- フィルター機能全体をレビューし、PR に Agent Merge を使用しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、更新した `main` から作成するブランチ上で、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価がないことを示す状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用します。また `quality-checks` のレポートを調整し、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー検証という複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は、要件とテスト範囲のレビューにそれらの結果をまとめました。

追加したテストは実際の不足を補うものである必要があります。QA の結果、新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更があれば検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、結果に影響します。このワークショップでは、指示を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを構成しました。カスタマイズは会話をまたいで再利用し、チームのニーズに応じて調整できます。指示は標準を定め、スキルは繰り返し作業をまとめ、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討に、**Interactive** は範囲を絞った変更を一緒に進めるときに、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

環境と同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、生成結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。次の CLI 機能も確認してみてください。

- `/review` で code review agent に変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別タスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを使い続けることで習熟できます。本番コード、趣味のプロジェクト、長年作ろうと思っていた小さなアプリなどで活用してください。チーム内で学びを共有し、互いの経験から学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、[Foundry Backer Concierge シリーズ][foundry]に進めます。他の環境と比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
