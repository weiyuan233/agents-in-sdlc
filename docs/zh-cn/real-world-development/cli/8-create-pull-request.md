---
title: "第 8 课 - 创建并合并功能拉取请求 (PR)"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置和测试都保存在同一分支上。现在将它们一起审查并创建拉取请求 (PR)。之前你手动合并了星级评分 PR；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常应将功能、说明更新、技能更新和 QA 智能体拆分到几个独立 PR 中。为简化本工作坊，我们在同一对话和分支中完成了整个筛选和质量工作流，并将所有更改放入这个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复持续集成 (CI) 检查失败、响应审查意见，以及在需要时变基；之后，如果仓库允许，则启用 GitHub 自动合并。

之前你一直手动选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似于“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、仓库设置或权限限制。如果流程受阻，请阅读报告的原因，并在仓库允许时手动完成已审查的合并。

## 总结与后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，探索 Copilot CLI 中用于上下文、模型、共享和可选云端委托的实用斜杠命令。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
