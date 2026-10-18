---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。创建 Tailspin Toys 存储库的个人副本，启动用于开发的 [codespace][codespaces]，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验室存储库

基于[模板][template-repository]创建存储库副本。新存储库包含实验所需的全部文件，后续练习都会在其中完成。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 选择实验室存储库页面上的 **Use this template** 按钮，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果参加由 GitHub 或 Microsoft 主办的活动，请按照导师提供的说明操作。否则，可以在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），后续实验会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 通过模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个工作坊都会使用这些议题，无需自行创建。

## 创建 codespace

接下来，使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待时，可以先了解 GitHub Copilot 的其他功能，接下来就会用到。

> [!CAUTION]
> 后续练习还会回到此 codespace。现在请将它保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可在 codespace 或本地[开发容器][dev-containers]中运行。两者都能确保环境已安装顺畅体验所需的全部先决条件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和 agent mode

要使用 GitHub Copilot Chat agent mode，IDE 中必须安装 GitHub Copilot Chat 扩展。如果使用 GitHub Codespace，通常已预装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息并按 Enter。这会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示登录 GitHub 账户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，应会看到 Copilot Chat 窗口。

## 总结

恭喜！你已创建实验室存储库的副本，并开始创建 codespace，后续编写代码时会用到它。

## 下一步

接下来开始使用 Copilot。继续学习[练习 1 - 自定义说明][next-lesson]，向 Copilot 介绍项目惯例。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
