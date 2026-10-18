---
title: "课程 1 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉其基本操作，并找到预先创建的筛选议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一款功能强大的代理式编码助手，可在终端中运行，让你通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它可以帮助分担任务、提出更改请求，并让工作保持专注。第一步当然是安装工具；好在可以使用熟悉的工具完成。

在本课中，你将：

- 使用 npm 安装 GitHub Copilot CLI；
- 使用 GitHub 账户完成身份验证；
- 信任工作坊存储库并尝试一段简短对话；
- 使用内置 GitHub MCP 服务器查找筛选议题。

## 场景

团队开始使用 AI 智能体处理不断增长的待办事项。Copilot CLI 将这种能力带入终端，而许多开发者日常都在终端中工作。本课将完成安装和身份验证，为后续工作坊中的使用做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 或 Homebrew 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本课将使用 npm。

1. 返回 Codespace 并打开终端。
2. 确认已安装 Node.js，且版本符合要求：

   ```bash
   node --version
   ```

   显示的版本应为 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应会显示版本号。

## 使用 GitHub 完成身份验证

首次启动时，Copilot CLI 会提示使用 GitHub 账户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果系统提示，请按照设备代码说明完成身份验证，并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认路径指向 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**。

> [!NOTE]
> 在 Codespace 中，可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有提示验证，就表示可以继续使用。

## 熟悉基本操作

在普通 shell 提示符下输入的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用箭头键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看当前安装版本支持的命令，然后按 <kbd>Esc</kbd> 关闭帮助屏幕。
3. 向 Copilot 提问，确认一切正常：

   ```plaintext
   这个项目有哪些关键文件？
   ```

4. 阅读回复，注意 Copilot 会先探索存储库再作答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 让 Copilot 查找筛选议题：

   ```plaintext
   使用 GitHub MCP，查找此存储库中标题为 "Allow users to filter games by category and publisher." 的议题。提供其 URL 和简短摘要。不要更改任何内容。
   ```

7. 打开 URL 并阅读议题。完成一项快速更改后会用到它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都会创建功能分支。

## 使用工作坊快捷方式

通常，Copilot CLI 在使用尚未获准的工具时会先征求许可。本工作坊会使用经用户批准的 `--yolo` 选项重新启动它，以便在 Codespace 中跳过这些确认提示，专注完成练习。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。仅在 Codespace 或虚拟机等隔离环境中使用，切勿将它设置为日常开发的默认选项。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

在本工作坊中，`--enable-all-github-mcp-tools` 会启用后续课程用于处理议题和拉取请求的 GitHub MCP 读写工具。Codespace 会限制对本地计算机的访问，但经过身份验证的 GitHub 资源仍是真实资源。发布或合并之前，请审查所有更改。

1. 使用 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再询问一个关于项目的简单问题，确认对话正常运行，然后使用 `/exit` 退出。

Copilot 会自动保存对话。稍后更改说明或添加智能体后，可以使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和下一步

恭喜！本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI；
- 使用 GitHub 账户完成身份验证；
- 信任了工作坊存储库，并尝试了一段简短对话；
- 使用内置 GitHub MCP 服务器找到了筛选议题。

接下来，开始[第一项聚焦更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
