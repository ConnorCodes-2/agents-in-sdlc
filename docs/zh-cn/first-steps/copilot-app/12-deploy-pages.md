---
title: "练习 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查其拉取请求，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，让所有人都能访问。Copilot 可以准备工作流并指导部署，但仍需自行审查更改并确认网站已上线。

在本练习中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已位于 GitHub 存储库中，但其他人仍需要一种简单的方式来体验它。使用 GitHub Pages 发布这个单文件网站，即可通过公共 URL 访问它，无需添加服务器或运行时依赖项。

## 让 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
为此存储库启用 GitHub Pages。创建一个 GitHub Actions 工作流，在需要时构建网站并将其部署到 GitHub Pages。如果尚不存在 README，请添加一个，介绍此应用以及如何在本地运行。如果需要更改，请创建拉取请求，并在必需检查通过后合并。持续监控部署，直到成功，然后报告在线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确告诉我需要批准或执行的操作；在网站上线之前，不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续之前，请审查差异、部署权限和 GitHub 的任何批准提示。

## 审查工作流并验证部署

1. 确认仅当存储库中尚无 README 时才会添加它。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在必需的拉取请求检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有在部署成功且在线网站正常加载后，才能认为任务已完成。

## 总结和下一步

Space Quiz 现已作为在线网站发布，并且你亲自验证了部署，而不是只依赖智能体的报告。继续学习[练习 13：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
