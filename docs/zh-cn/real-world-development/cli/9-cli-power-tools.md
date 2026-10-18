---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令支持高级功能、幕后信息和额外配置选项。你已经使用了 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来探索其他实用命令。

本课中，你将：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解如何使用 `/share` 导出或共享会话；
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在看看更多功能：管理上下文、切换模型、共享会话，以及可选地委派任务给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，也可以用斜杠命令自行检查或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及系统说明、工具、消息和剩余空间的上下文分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将填满时自动压缩上下文。如果要自行决定压缩时机，可使用 `/compact`。如果要切换到无关任务并开始新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 支持查看并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或者按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和共享经验有助于大家更好地使用 AI 工具。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或者发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先审查内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。请勿发布团队不打算共享的存储库或对话内容。

## 可选：扩展工作规模或委派任务

核心工作坊已经完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可将独立子任务分配给子智能体并行执行；
- `/worktree` 可为单独任务创建隔离的 Git 工作树；
- `/delegate` 可将任务交给 Copilot 云智能体异步处理，该智能体可能会创建拉取请求。

这些命令是可选的，因为它们可能会创建额外工作树或远程任务。试用前，请开始一个范围明确的新任务，并按照常规工作流审查结果。若想深入了解异步智能体工作，可以继续学习[云智能体工作坊][cloud-workshop]。

## 总结和下一步

使用 Copilot CLI 斜杠命令，可以配置 CLI、共享会话并查看幕后操作。本课中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解了如何使用 `/share` 导出或共享会话；
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多可用的斜杠命令和功能！接下来，[回顾所学内容][next-lesson]并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
