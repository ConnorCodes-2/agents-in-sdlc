---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你要求 Copilot 云智能体完成了三项与改进用户体验和添加功能相关的任务。云智能体以异步、自主的方式工作，但监控任务进度仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制面板中，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务，探索智能体执行的操作，甚至在会话进行期间引导它。

本课中，你将：

- 浏览智能体页面，监控云智能体任务；
- 在会话进行期间指导智能体，要求它添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队发现也正好可以添加浅色模式。既然已经要更新网站样式并添加切换功能，便可以一并实现浅色模式。现在需要指导智能体，在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含已分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务。查看 Copilot 执行过的操作，例如：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务的各项操作。

> [!NOTE]
> 具体显示哪些步骤取决于 Copilot 工作的进度和所采用的方法。

4. 还可以查看右侧显示的拉取请求 (PR) 窗格，了解 PR 和已更改的文件，以便继续监控。

## 指导云智能体

现在已查看正在执行的任务，请求 Copilot 在实现高对比度模式的同时也添加浅色模式。

1. 选择高对比度模式任务对应的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![控制面板中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示设置已完成且开始执行任务。出现类似以下的消息时，说明已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。还应添加一个用于切换模式的开关，让用户可以选择所需的显示模式。
    ```

    ![智能体页面中的云智能体任务屏幕截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会处理更新后的任务！收到消息时，如果 Copilot 正在执行某一步骤，它会在完成该步骤后将新请求纳入工作流程。

这需要几分钟，正好可以暂停片刻，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，也就是监控和指导 GitHub Copilot 云智能体任务的中心位置。通过此控制面板，可以跟踪所有进行中和已完成的任务、审查正在执行的工作，甚至重定向执行中的任务以调整范围或补充指导。

本课中，你：

- 浏览了控制面板和智能体页面，以监控云智能体任务；
- 重定向了执行中的会话，请求添加更多功能。

Copilot 正在完成无障碍功能。接下来，开始[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
