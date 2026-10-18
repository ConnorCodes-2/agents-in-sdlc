---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域创建的专用 AI 助手。可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为其提供专门的说明、最佳实践、编码模式和领域知识，从而帮助它更有效地完成特定工作。团队可以将专业知识整理成可复用的智能体，例如用于强制执行 [WCAG][wcag] 合规要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护统一测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description`，后面则是定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体和智能体技能的比较

自定义智能体与[智能体技能][agent-skills-concept]在用途上有一定重叠。两者主要通过 Markdown 文件定义，并指示 AI 如何执行操作。可以这样区分：**自定义智能体负责执行工作，技能则提供工具。**

自定义智能体拥有自己的上下文窗口，能够协调技能（甚至其他智能体）共同完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要通过测试和迭代找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期的用户反馈指出，当前暗色主题中，文本与背景颜色的对比度不足，难以阅读。为解决这一无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

由于无障碍至关重要，需要尽快实现此功能。本练习将使用自定义智能体生成所需功能。

本练习中，你将：

- 查看现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

我们已经为你创建了一个无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建一个新的无障碍功能分支（这样就能让筛选 PR 与练习 3 中的工作保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需项。

3. 浏览后续部分，查看其中列出的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 示例；
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 下定义的所有自定义智能体。选择自定义智能体后，聊天会话将使用该智能体的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。每项练习可能使用不同模式和模型；相关步骤会在适用时说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前使用的智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应提供一个可由用户设置的高对比度切换开关，并使用浏览器的 local storage 保存设置，使其在页面重新加载后仍然生效。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，包括文件路径和可供审查的差异。
6. 随着编辑逐步完成，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改存储库，包括 Astro 组件、CSS 和相关测试等现有文件。

下一练习中，将审查并指导此项进行中的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的专用 AI 助手。使用自定义智能体可以将团队的专业知识和标准整理为可复用的指导，帮助 Copilot 更有效地完成相应工作。

本课中，你了解了：

- 如何定义自定义智能体；
- 如何在 Copilot Chat agent mode 中使用自定义智能体。

接下来，[监控并指导智能体的工作][next-lesson]，查看更改并在同一会话中添加浅色模式切换功能。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
