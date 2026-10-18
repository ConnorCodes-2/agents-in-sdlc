---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个自定义智能体中，并为其定义清晰的 QA 流程。

本课中，你将：

- 了解自定义智能体如何使用说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件会说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择该角色。

你使用的各项自定义功能有不同作用：存储库说明描述团队标准；`quality-checks` 技能打包可重复使用的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告发现。QA 配置不会取代这些工具，也不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置将规定 QA 要执行的检查以及必须遵守的界限。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   创建一个名为 QA 的自定义智能体，文件路径为 .github/agents/qa.agent.md。它应根据议题和达成一致的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应使用通过、失败或受阻状态报告每项需求，并提供支持证据。更改实现代码前必须先询问，并且不得提交更改或创建拉取请求。

   现在只创建配置文件，以便我审查。
   ```

## 检查配置文件

使用新智能体之前，请先审查其配置，确认 Copilot 正确记录了 QA 流程和界限。这可以防止配置不完整或范围过宽的智能体在只要求验证功能时更改功能实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但填写后可作为智能体的显示名称。
3. 阅读配置说明，确认 QA 会从需求出发，遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在更改实现代码前先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏了任何职责或界限，请让默认智能体修改，然后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复同一筛选对话并选择 QA，这样它便能使用已有的议题和计划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建智能体，它可能不会出现在可用智能体列表中。它仍然可用，可以通过上面的命令启用。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 智能体会开始工作！
4. 完成后，阅读它提供的报告。

## 总结和下一步

你已在工作流中添加可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何使用说明、技能和 MCP 工具；
- 创建并检查了可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查了它的发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好，可供审查。接下来，[汇总这些内容并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
