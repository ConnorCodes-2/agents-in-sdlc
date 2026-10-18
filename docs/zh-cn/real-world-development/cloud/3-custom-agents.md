---
title: "练习 3 - 自定义智能体"
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

使用自定义智能体时，你将了解：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期的用户反馈指出，当前暗色主题中，文本与背景颜色的对比度不足，难以阅读。为解决这一无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

由于无障碍至关重要，需要尽快实现此功能。本练习将使用自定义智能体生成所需功能。

## 查看无障碍自定义智能体

我们已经为你创建了一个无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需项。

3. 浏览后续部分，查看其中列出的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 示例；
   - 常见问题和错误。

## 创建并分配议题

智能体控制面板是集中管理环境中所有智能体的地方。可以在此将任务分配给 Copilot 云智能体、监控任务，并重定向任务或提供更多指导。现在，将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配对话框中突出显示自定义智能体和 Accessibility agent 的屏幕截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的专用 AI 助手。使用自定义智能体可以将团队的专业知识和标准整理为可复用的指导，帮助 Copilot 更有效地完成相应工作。

本课中，你了解了：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，可以在智能体控制面板中[监控并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中准备使用自定义智能体][org-custom-agents]
- [在企业中准备使用自定义智能体][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
