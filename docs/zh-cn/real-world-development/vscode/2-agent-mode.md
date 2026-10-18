---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再继续更新……事情往往不止如此。

这正是 Copilot Agent Mode 的用武之地。

Copilot Agent Mode 可以在 IDE 中更加自主地工作。它会像开发者一样先探索现有项目结构，进行所需更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但 agent mode 和 cloud agent 针对不同场景。agent mode 在 IDE 中执行任务，便于快速反馈和互动；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调多文件更改；
- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能；
- 如何在将 Copilot Agent Mode 生成的更改合并到代码库之前审查代码和测试。

## 场景

游戏列表不断增长，需要让用户按类别和发行商筛选。上一练习中已经添加发行商帮助函数，现在使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先探索 Tailspin Toys 网站，了解它当前的功能。

此网站是一个具有开发者主题的桌面游戏众筹平台，可以列出游戏并显示游戏详情。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库——没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问帮助函数位于 `src/lib/`；交互功能使用标准 DOM API，通过小型、作用范围明确的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，便于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，当前应已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会看到类似以下的横幅，其中显示网站 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭对话框。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端显示的 localhost URL 链接，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示游戏标题、图片和说明的桌面游戏列表。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。每项练习可能使用不同模式和模型；相关步骤会在适用时说明。

网站的初始实现可以正常工作，但还需要添加新功能。先查看待办事项。在通过模板设置项目时，系统已自动创建一组议题；现在让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划和跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并不偏好任何特定模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此将其列入说明中，希望代码建议相对一致，提供良好体验。不过，由于大型语言模型具有概率性，收到的建议可能与文档示例不同。这完全正常，属于预期情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 使用以下提示询问 Copilot 存储库中的待办议题：

   ```plaintext
   请显示 GitHub 存储库中的待办事项，并根据对用户的价值帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还会根据它认为对用户最有用的程度为事项排序。

## 查看说明文件

让智能体生成代码前，先查看将用于提供上下文的说明文件。本练习将使用[用户界面 (UI)][ui-instructions]文件，其中包含网站功能开发指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中关于如何添加功能的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向不同文件类型专用说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这种方式有助于复用和拆分复杂说明，也可提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项单独更新：

- 在数据层 (`src/lib/`) 添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页，添加筛选 UI。

此外，测试必须通过后才能将更改合并到代码库。Copilot Agent Mode 可以处理这些任务！现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认在 Chat 视图的智能体下拉菜单中仍然选择 **Agent**。

   ![Chat 视图中智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择模型 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   请根据待办事项中的相关 GitHub 议题要求，为网站添加按发行商和类别筛选的功能。上一练习中已经添加了发行商帮助函数，因此请在需要时保留并完善现有工作，同时完成剩余的数据层、UI 和测试。完成前确保所有测试均通过。服务器已在运行，无需重新启动。
   ```

6. 观察 Copilot 如何先探索项目，找到目标功能相关文件，包括数据层帮助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库复杂度，这可能需要几分钟。期间编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有一个 **Keep** 或 **Undo** 按钮，因此无需在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，也可能看到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且没有发现错误。

   ![Copilot Agent Mode 完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习中，你了解了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解到：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能；
- Copilot Agent Mode 如何探索项目、识别相关文件并协调多文件更改；
- 如何在将 Copilot Agent Mode 生成的更改合并到代码库之前审查代码和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习——实现分页

游戏列表不断增长后，需要支持分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是固定每页大小；
- 编写提示时提供必要指导，让 Copilot 按预期实现；
- 可能需要与 GitHub Copilot 反复迭代，请求更改并补充上下文。这是使用 Copilot 时的常见流程！

## 资源

- [Copilot 的 ask、edit 和 agent mode：功能和适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
