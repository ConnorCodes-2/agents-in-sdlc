---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキル、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能、指示ファイルの更新、スキルの更新、品質保証 (QA) プロファイル、テストを同じブランチに保存しました。内容をまとめてレビューし、プルリクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常なら機能、指示の更新、スキルの更新、QA エージェントは別々の PR にします。このワークショップでは手順を簡略化するため、フィルター機能と品質ワークフローを同じ会話とブランチにまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの自動化の流れを学びます。
- 変更全体と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルター機能の開発を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを許可する権限を保ちながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている項目に対応します。失敗した継続的インテグレーション (CI) チェックを修正し、レビューコメントに対応し、必要に応じてリベースして、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge を使う場合も、まず作業をレビューし、準備ができたと判断してから有効にしてください。

## Agent Merge で PR を管理する

コード全体をレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功するとマージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリの設定が許す場合は、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テスト、検証に加えて、プルリクエストのプロセスも自動化しました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使う[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
