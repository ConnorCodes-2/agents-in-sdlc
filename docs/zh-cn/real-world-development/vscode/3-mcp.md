---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：agent mode][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

刚刚已使用 agent mode 构建筛选功能。在创建拉取请求之前，应先确认功能在浏览器中正常运行。无需自己逐一操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 使用真实浏览器测试功能，然后发布分支并创建 PR。

本练习中，你将：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 添加新工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当 agent mode 能够访问编辑器之外的内容时，其功能会大幅增强。Copilot 通过 Model Context Protocol (MCP) 实现这一点：这是一种让智能体与外部工具和服务通信的标准方式。

![agent mode 的内部工作方式及其如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，智能体可以实时访问最新信息（使用资源）并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器在智能体与外部工具和服务（例如现有 API 或 npm 包等本地工具）之间管理通信。每个 MCP 服务器都代表智能体可以使用的一组工具和资源。

常用的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。智能体可以使用它创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。智能体可以访问网页、填写表单并选择按钮。

还有许多其他提供不同工具和资源的 MCP 服务器。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，用于提升生态系统的可发现性并促进贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，仔细审查其源代码、验证发布者并考虑安全影响。仅使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该会看到已为你配置的 `github` 服务器：

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

此 `github` 配置来自项目模板，因此 Copilot 能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 通过 HTTP 连接，Copilot 首次使用工具时使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件如下所示：

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

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数会让 Playwright 在后台运行浏览器，而不显示窗口；Codespace 没有桌面环境，因此必须使用此选项。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装所需浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要某服务器的工具时，VS Code 会启动它，并询问是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择该操作立即启动服务器，以确认配置正确。
2. 如果 VS Code 要求确认是否信任此服务器，请审查配置并选择信任，以便启动服务器。

手动启动服务器是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操作真实浏览器。无需自己逐步操作应用，智能体即可打开网页、导航、应用筛选条件并读取结果，然后总结观察情况。无需离开对话，就能快速确认功能是否按预期运行。

Playwright MCP 服务器实际使用页面的[无障碍树][playwright-mcp-server]，而非屏幕截图。这意味着智能体根据有标签的结构化元素（按钮、链接和列表项）进行推理，方式与辅助技术类似，因此快速功能检查也能顺便检查基本无障碍行为。

连接服务器并启动应用后，请 Copilot 测试刚才构建的筛选功能：

```text
使用 Playwright MCP 服务器在 http://localhost:4321 打开运行中的应用，并验证新游戏筛选功能：

1. 打开游戏页面，记录显示的游戏数量。
2. 应用类别筛选条件，确认列表仅显示该类别的游戏。
3. 清除筛选条件，再应用发行商筛选，确认列表仅显示该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时符合这两个条件。

报告每一步的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步执行检查，然后报告结果。请结合议题中的验收标准审查其总结。如果结果有问题，请继续提问或让 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示 Copilot 所做更改的屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。既然已经在使用 GitHub Copilot，不妨让它来创建 PR！它可以找到相关议题，并创建与该议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选有关的议题。为当前分支创建新的拉取请求，并将其关联到正确的议题。
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。Copilot 首次使用 GitHub 工具时，可能会提示使用 GitHub 完成身份验证；按照提示操作即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接，审查拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 添加新工具；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用它在发布筛选功能前通过浏览器进行手动测试；
- 发布了分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
