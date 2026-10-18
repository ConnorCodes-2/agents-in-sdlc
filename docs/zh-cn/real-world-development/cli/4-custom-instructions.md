---
title: "课程 4 - 使用自定义说明指导 Copilot"
description: "探索存储库说明、添加文档标准，并将该标准用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。若任务需要以特定方式完成，就要确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅描述希望生成什么代码，也说明代码应该如何组织。现在筛选功能已经完成，接下来查看 Copilot 使用的说明，添加文档标准，并将它应用于代码。

本课中，你将：

- 了解如何通过存储库说明和按路径限定的说明文件向智能体提供上下文；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发实践准则和要求：

- 注释应说明意图和不明显的设计决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值，包括其中可注入的 `db` 参数；
- 可复用的 Astro 组件应记录其 `Props` 接口契约，代码更改时也应同步更新注释；
- 现有格式和 Lint 指导应继续保留。

通过说明文件，可以让 Copilot 获取这些信息，并按照团队实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格与要求，从而生成更相关的建议和代码片段。可以指定编码惯例、库，甚至希望添加的注释类型。说明既可适用于整个存储库，也可针对特定文件类型或任务。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次向 Copilot 提交存储库请求时都会发送的单个说明文件。此文件应包含大多数请求都适用的项目级上下文。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 不同环境对其他说明格式的支持有所不同。依赖某种特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 浏览此项目中的自定义说明文件

起始项目中已包含一组说明文件。先看看现有内容，再进行更改以观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此存储库中的所有 Copilot 交互都会应用这些说明。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了用于确定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他说明文件和现有项目文件的链接。这样可以将较大的说明拆分为可复用的小文件，并提供可供 Copilot 参考的示例。

## 更新说明文件以符合团队指导

现有文件是一个不错的起点，但仍有改进空间。现在修改核心文件 `copilot-instructions.md`，确保生成的 TypeScript 代码添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应位于第 35 行附近）。
2. 在此部分末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在会话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了说明和代码指导。请根据这些指导更新你生成的代码。
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。检查新生成的 TSDoc 注释，确认其准确说明了代码。

## 总结和下一步

你已了解 Copilot CLI 如何读取说明文件，并将一项新标准用于筛选功能。具体来说，你：

- 了解了如何通过存储库说明和按路径限定的说明文件向智能体提供上下文；
- 更新了说明文件，确保遵循编码标准；
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 Lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
