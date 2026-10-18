---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已经手动验证代码可以正常运行，并使用说明文件确保代码符合标准。接下来还需要测试、Lint，以及持续集成 (CI) 的其他工作。

**智能体技能**很适合处理这些任务！技能可以帮助 Copilot 正确运行此类操作。

本课中，你将：

- 探索现有的 `quality-checks` 技能；
- 自定义技能结果的格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、Lint 和类型检查。团队希望改进报告格式，让结果更易阅读。

## 说明、脚本和资源

智能体技能会打包可复用的任务说明、可执行脚本和支持资源，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含一个 `SKILL.md` Markdown 文件。该文件包含说明技能名称和描述的 frontmatter、技能概述，以及关于何时调用技能的指导。文件夹中还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外的文件夹和文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它会使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，成为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来看看 Tailspin Toys 团队为运行单元测试、Lint 和类型检查编写的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述有助于 Copilot 判断何时调用此技能。
3. 阅读说明，了解它们如何引导 Copilot 执行测试和 Lint。
4. 注意此技能目前还没有 **Results output formatting** 部分。

## 修改前运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言提出请求。现在请 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、Lint 和类型检查。
   ```

3. 查看最后生成的报告。

## 自定义报告

现在希望报告说明运行了哪些检查、是否成功，以及工具实际报告的内容。接下来更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## 结果输出格式

   完成后，报告运行的每条命令，并说明它通过、失败还是被阻止。仅在工具提供数据时，才包括测试数量、耗时、错误、警告和其他指标。对于失败或被阻止的检查，指出下一步操作；切勿将跳过或未完成的检查描述为已通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

更新后，看看实际效果！Copilot CLI 无需重启对话即可重新加载经过编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、Lint 和类型检查。
   ```

3. 查看最后生成的报告，并与第一次报告进行比较。

## 总结和下一步

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于运行单元测试、Lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果格式；
- 重新加载并运行了技能。

这项更改会与筛选功能一同包含在功能 PR 中。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接操作网站。

## 更多技能示例

以下社区示例仅供参考，不是额外的任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
