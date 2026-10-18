---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并创建和合并第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，接下来要为项目进行第一项更改。先从小处着手：游戏数据中已经包含星级评分，但主页上的游戏卡片还没有显示。请智能体显示评分，审查更改，并将其作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支中开始一次聚焦的 Copilot 对话；
- 让智能体对项目进行一项小更改；
- 使用 `/diff` 审查更改；
- 运行应用，在转发的浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，让智能体在每张卡片上显示现有评分。这是一项小而独立的更改，非常适合第一次使用 Copilot CLI。

## 对话的组成

**对话**是使用 Copilot CLI 完成任务的空间。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中会看到三项内容：你发送的提示和智能体的回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

现在开始一段新对话，实现这个功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确认已启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片中显示每款游戏的 starRating（满分 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情的文件，并编写所需代码。现在已经使用 Copilot CLI 添加了一项新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使是小更改也不例外。现在直接在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在评分存在时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行检查的结果，并让它修复任何失败。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有概率性，而非确定性。生成的代码可能不同。请审查其行为，而不是期待某种特定实现。

## 检查更改

不能只阅读代码就假设它能正常工作。让 Copilot 启动网站，然后通过 Codespaces 转发的端口在浏览器中检查更新后的用户界面 (UI)。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已经完成！接下来创建拉取请求 (PR)，将代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   使用合适的提交消息，提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查已更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和下一步

恭喜！已使用 GitHub Copilot CLI 发布了第一项更改！具体来说，你：

- 在功能分支中开始了一次聚焦的 Copilot 对话；
- 让智能体对游戏卡片进行了一项小更改；
- 使用 `/diff` 审查了更改；
- 运行应用，在转发的浏览器中确认了星级评分；
- 创建并合并了第一个拉取请求。

接下来，[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
