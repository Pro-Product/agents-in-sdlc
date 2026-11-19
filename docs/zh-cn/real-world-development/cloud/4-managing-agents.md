---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项不同任务，以改进用户体验并添加功能。云智能体虽然可以异步、自主工作，但监控这些任务仍然很重要。

可以通过多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 Mission Control 中，可以查看所有包含未关闭拉取请求 (PR) 的智能体任务，了解执行的操作，甚至在会话进行时引导它。

本练习将带你：

- 使用智能体页面监控云智能体任务。
- 在会话进行中引导智能体添加功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，就适合同时实现这个功能。现在引导智能体，在添加高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前工作状态和处理方式而异。

4. 还可以查看右侧的拉取请求 (PR) 面板，在此查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

了解当前正在执行的任务后，让 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择负责添加高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。看到类似下方的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟，可以借此暂停并回顾目前学到和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以在 Mission Control 中跟踪所有正在进行和已完成的任务、查看执行情况，并重定向正在运行的任务以调整范围或提供额外指导。

本练习介绍了以下概念：

- 使用 Mission Control 和智能体页面监控云智能体任务。
- 重定向正在运行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
