---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码只是开发工作的一部分。还需要处理数据和外部服务，甚至让 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 的内置功能，让 Copilot 使用更多工具和服务。

本课中，你将：

- 了解 Model Context Protocol (MCP) 及其在 Copilot CLI 中的用法；
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器；
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更改，仍需实际与 UI 交互。现在让 Copilot 像用户一样使用网站，进一步自动化更改验证流程，提高对更新结果的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 让 AI 智能体能够与外部工具和服务通信。通过 MCP，智能体可以实时获取最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器都代表智能体可以使用的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助用户发现并为 MCP 生态系统作出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，请检查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过此设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前已手动确认功能是否正常运行。现在使用 Copilot 中的 Playwright MCP 服务器执行相同的验证。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用并使用 Playwright MCP，根据议题和我们的计划检查筛选功能。告诉我哪些功能正常、哪些不正常，不要进行更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，因为它往往会自行判断。不过，既然知道应该使用哪个服务器，明确指出也有助于确保结果一致，并节省一些令牌。

2. 观察 Copilot 列出在浏览器中验证功能的各项步骤。
3. 阅读报告，确认所有功能均符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你报告结果。

## 总结和下一步

恭喜！你已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器探索功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 及其在 Copilot CLI 中的用法；
- 如果 Playwright MCP 服务器尚不可用，则添加了该服务器；
- 让智能体操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
