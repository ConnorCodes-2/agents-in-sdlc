---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取相关上下文。[说明文件][instruction-files]可提供相关指导，帮助 Copilot 理解代码应该如何生成。

本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定的说明文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在现有说明指导下，生成筛选功能的第一个数据切片（发行商帮助函数）；
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库标准；
- 再次发送相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设置的所有标准。Copilot 具有概率性。本练习的重点是观察更新说明后行为的*趋势*变化，而不是让输出与示例逐字相同。

## 说明文件

### 场景

Tailspin Toys 制定了一系列开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 代码文档应采用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

通过说明文件，可以让 Copilot 获取这些信息，并按照团队实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格与要求，从而生成更相关的建议和代码片段。可以指定编码惯例、库，甚至希望添加的注释类型。说明既可适用于整个存储库，也可针对特定文件类型或任务。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次向 Copilot 提交存储库请求时都会发送的单个说明文件。此文件应包含大多数聊天或 CLI 请求都适用的项目级上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的说明文件。可以为 TypeScript、Astro 等语言，或 UI 组件和单元测试等任务编写指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

本练习不会深入讲解如何编写说明文件，但示例项目展示了有代表性的方法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如正在构建的内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供说明。
- 使用自然语言，让指导清晰易懂，并说明代码应该和不应该采用的形式。

说明文件没有唯一正确的写法，使用 AI 也一样。通过实践找到适合项目和场景的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览此项目的说明时，会看到针对多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面中的入口各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关工作流会在适用时介绍这些功能。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花些时间查看此存储库附带的说明文件：其中包括一个核心 `copilot-instructions.md`，以及多个适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些说明适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览内容。注意其中包括 Astro 文件、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看文件顶部的 `applyTo` 字段；它设置了一个相对于存储库根目录的 glob，用来确定说明适用的文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目关于创建单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以把较大的说明拆分为可复用的小文件，并提供 Copilot 可参考的示例。（这些路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释标题的规则。

## 创建分支

现在为更改创建分支，遵循最佳实践。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。每项练习可能使用不同模式和模型；相关步骤会在适用时说明。

要观察自定义说明的影响，先在现有说明文件的基础上发送提示。稍后更新说明后，再发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 仅获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道帮助函数应位于何处。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问帮助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改帮助函数文件和测试。
6. 注意，建议的帮助函数是一个类型化函数，它以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层惯例。
7. 注意，建议的代码**没有** TSDoc 文档注释或文件级注释标题。

> [!CAUTION]
> 由于 Copilot 具有概率性，即使没有明确要求，它也可能添加文档注释。如果发生这种情况也没有问题；本练习的重点仍是观察更新说明后的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于提供 Copilot 的项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。查看其中记录的编码标准；目前还没有代码文档规则，因此生成的帮助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。存储库级文件适合放置刚添加的文档注释规则等全局项目标准。

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除缓冲区。
2. 再次单击 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问帮助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

4. 注意，建议的文件现在以类似以下的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问帮助函数。
    * 提供用于从数据库检索发行商信息的函数。
    */
   ```

5. 注意，建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商及其 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

现在已引导 Copilot 遵循新的项目标准，并将其应用于下一练习会继续使用的实际代码。

## 提交说明并推送分支

说明文件与存储库中的其他文件一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认说明更新和帮助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为帮助函数基础更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他辅助更改（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动创建，但本练习将让 Copilot 查找议题、创建可关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用时，VS Code 可能会提示登录 GitHub；按照提示操作即可。后续练习将介绍此连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选择了 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并基于当前分支创建 PR，说明其中包含说明更新和新的发行商帮助函数基础：

   ```plaintext
   查找与更新说明文件有关的议题。基于当前分支创建新 PR，说明此 PR 会关闭该议题，并且还添加了后续筛选功能工作所需的发行商帮助函数基础。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 提示允许其代表你执行 GitHub 操作时，请先检查命令，再按需选择 **Allow**。
6. 创建 PR 后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码保持最新。
   ```

7. Copilot 提示允许其执行 GitHub 操作和 shell 命令时，请先检查命令，再按需选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求！

## 总结和下一步

你已了解 Copilot 如何读取项目说明，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在*现有*说明下生成的内容；
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库标准；
- 再次发送相同提示，观察建议的代码如何采用新标准；
- 将说明更新和帮助函数基础更改提交到 `main`，以便下一练习继续使用。

接下来，在 [agent mode][next-lesson] 中使用这些说明，让 Copilot 跨文件添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
