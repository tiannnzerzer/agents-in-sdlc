---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前面几项练习中，你让 Copilot 云智能体承担了三个改善用户体验和添加功能的任务。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心中，可以查看所有带有未关闭拉取请求（PR）的智能体任务、查看已执行的操作，还可以引导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。网站样式和切换功能已经在更新，因此加入这项功能很合理。现在，你将引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 工作进度和采用的方法而变化。

4. 同时留意右侧出现的 PR 面板。你可以在那里查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

查看活动任务后，接下来要求 Copilot 在实现高对比度模式的同时加入浅色模式开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![智能体控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示设置已完成并开始执行任务。之后会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。完成你发送消息时正在执行的具体步骤后，它会将新请求加入工作流程。

这同样需要几分钟。等待时，可以停下来回顾目前学到和体验过的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。使用此控制中心，可以跟踪所有活动和已完成的任务、审查正在执行的工作，还可以重定向进行中的任务以调整范围或提供额外指导。

本课中，你：

- 使用智能体控制中心和智能体页面监控了云智能体任务。
- 重定向了正在进行的会话，请求添加额外功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

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
