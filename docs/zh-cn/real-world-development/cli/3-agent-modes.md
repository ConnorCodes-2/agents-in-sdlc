---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据议题构建筛选功能，再在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已为项目添加一项小功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作方式，帮助以正确的方式构建所需功能。本课将介绍一系列智能体驱动开发流程中的第一步：从议题开始构建新功能，确保代码有效、功能符合预期，并最终成功合并到项目。

> [!NOTE]
> 在后续功能工作流中，将继续使用同一个对话和分支。通常会针对不同文件类型使用不同的分支或 PR；本工作坊会采用快捷方式，帮助聚焦核心概念。

本课中，你将：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 在 Autopilot 模式中实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了这项功能，但组合类别等细节需要在编码前确定。使用 Plan 模式协商这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变基本原则，反而让这些原则更加重要。大多数开发者采用的流程大致如下：

1. 从描述待办事项的已创建议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试，验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程与上述步骤类似。

遵循这类标准流程，可以确保 AI 生成的代码符合既定需求，并经过与手写代码相同的审查过程。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你和智能体协同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，并且不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试，并持续迭代，直到任务完成。

先在 Plan 模式中查看计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选功能之前，请返回 Codespace 并确认存储库和终端均已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动后再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

请注意，此时提示开头会出现 `#7`（或类似编号）。`#` 可将 GitHub 议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划功能可以在交给 Copilot 之前先确定实现方案。复杂任务通常都值得花一些时间进行规划。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   为实现此功能制定一个计划。
   ```

Copilot 会开始制定计划！它会先探索项目，再确定适合的实现方案。

3. 期间 Copilot 可能会询问筛选功能的工作方式。根据你的偏好回答；没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动浏览计划并进行审查。
6. 如果计划的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划完成并通过审查后，即可开始实现！使用 Autopilot 让 Copilot 执行。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或所安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，Copilot 可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

生成的代码和其他代码一样，需要在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题以及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并让 Copilot 解决所有失败。

## 探索新功能

代码看起来不错，但是否真的能运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别和发行商筛选，以及计划中确定的组合筛选方式。
4. 确认重置和无结果状态符合议题要求及你的决定。
5. 返回 Codespace，让 Copilot 停止刚才启动的开发服务器。

## 总结和下一步

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题开始了新的 Copilot 对话；
- 在 Plan 模式中明确了需求；
- 在 Autopilot 模式中实现了新功能；
- 审查了代码；
- 在转发的浏览器中手动验证了功能。

接下来，深入了解代码生成方式，确保代码遵循已记录的实践：[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
