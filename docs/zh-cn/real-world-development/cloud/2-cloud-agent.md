---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期积压的功能请求。GitHub Copilot 云智能体可以自主执行代码更新、添加功能等任务。完成工作后，它会生成一个草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体处理，加快开发流程，让开发者专注于更重要的工作。

本练习中，你将使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解清晰限定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初承包开发网站的团队没有留下合适的文档——实际上完全没有文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望游戏详情页显示相关游戏——即同一类别中的其他游戏——帮助用户继续浏览。他们暂时不需要完善的实现，只想有可用于验收测试 UX 的内容。此需求目前有阻碍，但还有优先级更高的议题。

这两项任务都很容易被推迟，因此非常适合交给 Copilot 云智能体。云智能体可以在后台异步工作，让开发者处理其他任务，之后再回来审查工作并确认结果符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。与人类开发者协作的方式有很多种，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。任务进行中或完成后，都可以查看 Copilot 会话。解决方案准备好审查时，Copilot 会在拉取请求中标记你。

## 明确限定说明范围的重要性

GitHub Copilot 没有魔法，不能让你免于思考。即使是看似简单的操作，深入分析后也可能发现复杂性。因此，[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]非常重要。将它视作 AI 结对编程助手：分阶段工作、学习、尝试并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其引导 Copilot 的方式。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的效果；如果没有，也可以现在快速查看。在将任务分配给 Copilot 云智能体之前，先只读查看存储库中包含的说明文件，以便稍后观察它们的影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 惯例和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

接下来将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 惯例和文件注释标题。这些要求来自你已查看的说明文件。后续练习审查 PR 时还会再次提到它们。

## 为 Copilot 云智能体设置开发环境

无论由谁执行，编写代码通常都需要特定环境并运行一些设置脚本，确保环境状态良好。将任务分配给 Copilot 也一样，因为它会像软件工程师一样执行工作。

云智能体使用 [GitHub Actions][github-actions] 设置工作环境。可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]来自定义环境，在云智能体开始工作前运行所需步骤，让它获得必要的开发工具和依赖项。此工作流已预先配置，以简化实验流程并提供学习机会，确保 Copilot 能使用 Node.js、项目依赖项、端到端测试的浏览器二进制文件，以及已迁移并填充数据的 Astro 单应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为步骤所需的最低级别。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试可以使用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个需要注意的要点：

- 它仅包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 注意其中的 `workflow_dispatch` 触发器。这允许你从存储库的 **Actions** 选项卡手动运行工作流，便于测试工作流，而无需等待 Copilot 启动它。

## 添加文档

虽然大家都知道文档很重要，但大多数项目的文档要么已经过时，要么完全缺失。这类技术债务经常被忽视，会降低工作效率，也让维护代码库或让新开发者加入团队变得更困难。幸运的是，Copilot 很擅长编写文档，因此非常适合将此议题交给 Copilot 云智能体处理。它会在后台生成所需文档，后续练习中可以回来审查。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    我们组织要求在适当时为函数和方法添加 TSDoc 文档注释。遗憾的是，近期更新没有遵循这一标准。我们需要更新现有代码，确保在文档注释有助于说明行为时添加注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等一两分钟并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。Copilot 正在准备环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 审查待办事项列表及 Copilot 即将完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 滚动查看实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适计划，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一是能够异步执行任务，让你继续处理其他工作。现在利用这一点，再给云智能体分配一项任务，同时开始编写代码为应用添加功能。

## 为游戏详情页添加相关游戏版块

如前所述，GitHub Copilot 云智能体的优势之一是可以拆分工作，让你和它各自处理不同的任务。即使设计团队所需的相关游戏版块不会花太久时间，实现它仍会占用可用于其他工作的时间。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    我们希望在每款游戏的详情页上显示相关游戏，帮助用户发现更多游戏。设计团队希望探索 UX 并进行一些验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问帮助函数，返回与给定游戏属于同一类别的其他游戏，但不包括该游戏本身
   - 在游戏详情页中使用该帮助函数显示“相关游戏”版块
   - 处理游戏没有相关游戏的情况
   - 为新帮助函数创建单元测试
   - 创建 PR 之前，确保所有测试均通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论上会出现 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求！Copilot 云智能体的工作方式类似于软件工程师，因此无需持续监控；等它完成后再审查即可。接下来，了解如何创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步执行任务。可以用 Copilot 处理技术债务、创建新功能，或帮助将代码从一个框架迁移到另一个框架。

本课中，你了解了：

- 自定义生成代码的环境；
- 确保安全地执行操作；
- 清晰限定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
