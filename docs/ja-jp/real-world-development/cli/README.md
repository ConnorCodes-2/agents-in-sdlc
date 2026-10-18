---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys の変更を 2 件構築、検証、提出します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行して、外部ツールにも接続できます。コマンドラインから作業できるため、グラフィカルエディターに切り替える必要はありません。

このワークショップでは、GitHub Codespaces でプロジェクトを準備して Copilot CLI をインストールした後、星評価の変更を行い、ブラウザーで確認して最初のプルリクエスト (PR) を手動でマージします。続いて、フィルター機能を Plan モードで計画し、Autopilot モードで実装して、Interactive モードでレビューします。リポジトリの指示を更新し、既存の `quality-checks` スキルを調整し、Playwright MCP サーバーと QA カスタムエージェントで検証します。最後に、フィルター機能の変更をレビューし、Agent Merge で PR をマージして、コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認します。このワークショップでは、星評価の変更と、指示の更新、スキルの変更、QA プロファイル、テストを含むフィルター機能の変更について、2 件の PR を作成します。フィルターと品質確認の作業では、同じ会話とブランチを引き継ぎます。また、[Copilot CLI と Foundry を使うオプションの 3 モジュール シリーズ][foundry]では、モデルを準備し、カタログに基づくエージェントをデプロイして、サイトに接続します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本的な操作を確認してフィルター Issue を見つける |
| [2. 星評価を追加する][ex2] | 最初の変更 | 既存の評価と未設定時の表示を確認し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | 計画と実装 | Plan で方針を決め、Autopilot で Issue の機能を実装する |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 指示を確認、更新し、フィルター機能に適用する |
| [5. エージェントスキル][ex5] | スキル | 再利用可能な品質チェックのスキルを調整して実行する |
| [6. Playwright MCP による検証][ex6] | 外部ツール | Playwright MCP を使い、ブラウザーで機能を確認する |
| [7. QA エージェントの作成と使用][ex7] | エージェント | QA カスタムエージェントを作成して機能を検証する |
| [8. 機能のプルリクエストを作成してマージする][ex8] | プルリクエスト | 変更をレビューし、Agent Merge でマージする |
| [9. Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、委任に使うコマンドを確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフローを振り返り、次の学習に進む |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
