---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云智能体便会在后台处理：探索存储库、进行更改并创建拉取请求。与此同时，你可以继续处理其他工作。

在本系列练习中，将添加云智能体要遵循的自定义说明，然后分配 GitHub 议题让它实现功能。你还将查看并使用自定义智能体来引导它的工作方式，通过智能体控制面板监控并指导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体要遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并指导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查拉取请求、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为账户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，可用于完成所有工作流。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 计划均可使用云智能体。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用此功能。

> [!NOTE]
> 本工作流不介绍 MCP。若要了解如何将 MCP 服务器与 Copilot 搭配使用，请参阅 [CLI 工作流](../cli/)或 [VS Code 工作流](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
