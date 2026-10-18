---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウド上で非同期に作業できる機能です。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、ほかの作業を続けられます。

この演習では、cloud agent が従うカスタム命令を追加してから、GitHub Issue を割り当てて実装を任せます。作業のレビューやカスタムエージェントの活用、agents ダッシュボードからのセッションの監視と方向付けを体験し、最後にプルリクエストをレビューして成果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | Issue を Copilot cloud agent に割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視し、方向付ける |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次のものを準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれるため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise の場合、管理者が組織で有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/) を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
