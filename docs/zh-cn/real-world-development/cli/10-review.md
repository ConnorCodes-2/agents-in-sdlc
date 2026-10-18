---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连贯工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到预先创建的筛选议题；
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查；
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、Lint 和类型检查；
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并通过真实浏览器探索筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、测试覆盖率、技能结果和浏览器证据；
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索了用于上下文、模型、共享和可选云委派的斜杠命令。

## 已完成的交付

本工作坊包含两个 PR 阶段，每项工作都从已更新的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加关联测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化工作坊，这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、Lint 和类型检查，并按新格式报告结果。QA 在 PR 之前结合这些结果审查了需求和测试覆盖率。

新增测试应弥补实际缺口；如果 QA 运行后确认无需增加测试，也可能是正确的。授权合并前，请审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义设置，并根据团队需求进行调整。说明定义标准，技能描述可重复执行的任务，自定义智能体定义专门角色，而 MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 提前规划方案，使用 **Interactive** 处理需要密切参与的聚焦更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可以选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清楚说明希望构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 Copilot CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获取不同视角；
- 使用 `/fleet` 并行协调相互独立的子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提高工具使用能力的最好方式就是持续使用它！可以用 Copilot 处理生产代码、个人项目，或者多年来一直想构建却还没开始的小应用。与团队分享经验，也从团队成员的经验中学习。最后，别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若要了解其他环境，也可以浏览 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
