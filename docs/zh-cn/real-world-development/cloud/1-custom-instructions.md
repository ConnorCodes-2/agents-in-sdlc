---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定的说明文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会使用“更新前/更新后”提示进行对比。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示说明的影响。稍后审查云智能体创建的拉取请求时，可以看到说明文件的作用。

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

本工作坊不会深入讲解如何编写说明文件，但示例项目展示了有代表性的方法：

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
2. 查看文件中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些说明适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览内容。注意其中包括 Astro 文件、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看文件顶部的 `applyTo` 字段；它设置了一个相对于存储库根目录的 glob，用来确定说明适用的文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目关于创建单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以把较大的说明拆分为可复用的小文件，并提供 Copilot 可参考的示例。（这些路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释标题的规则。

## 添加新的存储库标准

接下来需要编辑的内容只有一项：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释标题记录代码文档。后续练习中，云智能体处理分配的议题时会采用此规则。

编辑前，先创建用于提交更改的分支（云智能体会读取议题目标分支上的说明，但仍建议单独提交本次更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于提供 Copilot 的项目级信息。添加编码标准可以改进代码建议。

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

## 提交、推送并合并说明更新

云智能体会读取议题目标分支上的说明。下一练习中将从 `main` 分支创建分支，因此必须先将说明更新合并到 `main`，云智能体才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中将议题分配给 Copilot 时，云智能体便会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，也可以跳过创建分支，并直接提交到 `main`。这里创建分支是为了演示真实项目中的常见做法。

## 总结和下一步

你已了解 Copilot 如何读取此项目的说明文件，并在 `.github/copilot-instructions.md` 中添加了一项全局存储库标准。后续练习中可以在云智能体创建的拉取请求里看到该标准的效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
