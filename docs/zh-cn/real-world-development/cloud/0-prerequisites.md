---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。创建 Tailspin Toys 存储库的个人副本，并启动一个 [codespace][codespaces]，以便编辑说明文件并审查云智能体生成的工作。

## 设置实验室存储库

基于[模板][template-repository]创建存储库副本。新存储库包含实验所需的所有文件，整个练习过程中都会使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面上，选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果参加由 GitHub 或 Microsoft 主办的活动，请按照导师提供的说明操作。否则，可以在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），后续练习会用到。

## 创建 codespace

接下来，使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待时，可以先了解 GitHub Copilot 的其他功能，接下来就会用到。

> [!CAUTION]
> 后续练习还会继续使用此 codespace。现在请将它保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可在 codespace 或本地[开发容器][dev-containers]中运行。两者都能确保环境已安装顺畅体验所需的全部先决条件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

现在已创建实验室存储库副本，并开始创建 codespace。后续与 Copilot 云智能体协作时会用到它。

## 下一步

接下来，添加云智能体要遵循的自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
