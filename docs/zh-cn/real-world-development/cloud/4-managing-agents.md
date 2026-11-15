---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你让 Copilot cloud agent 处理了三个不同任务，以改进用户体验并添加功能。cloud agent 虽然会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，还可以引导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，将浅色模式也纳入其中很合理。你将引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 为 Copilot 创建环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 还要查看右侧显示的 PR 窗格。在此可查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

查看活动任务后，请求 Copilot 在实现高对比度模式的同时也加入浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍智能体会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Agents 页面中用于引导 Copilot 活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新要求加入工作流程。

这同样需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，它是监控和指导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪所有活动和已完成的任务、审查工作，也可以重定向正在进行的任务以调整范围或提供更多指导。

你探索了以下内容：

- 使用任务控制中心和智能体页面监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
