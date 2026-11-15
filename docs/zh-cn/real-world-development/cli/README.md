---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "在 GitHub Copilot CLI 中学习、验证和交付两个 Tailspin Toys 变更，同时掌握 Copilot CLI 模式、自定义能力、MCP 工具和拉取请求自动化。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 作为代理式编码助手带入终端。它可在命令行中探索代码库、生成代码、运行命令，并连接外部工具——一切都在命令行内完成，因此你可以在不切换到图形编辑器的情况下保持工作流。

该工作坊遵循一个连续的 Tailspin Toys 工作流：

1. 在 GitHub Codespaces 中准备项目，安装 Copilot CLI，并快速熟悉环境。
2. 完成一个聚焦的星级评分改动，在浏览器中审查，并手动合并你的第一个拉取请求 (PR)。
3. 从筛选议题开始，在 Plan 模式中定义方案，在 Autopilot 模式中构建功能，然后在 Interactive 模式中审查。
4. 更新仓库说明，并将其应用到筛选工作中。
5. 自定义现有的 `quality-checks` 技能，并用它运行项目检查。
6. 添加 Playwright 模型上下文协议 (MCP) 服务器，并用它在浏览器中验证筛选功能。
7. 创建质量保证 (QA) 自定义智能体，并用它审查需求、覆盖范围和验证证据。
8. 审查完整的筛选功能更改，并对第二个 PR 使用 Agent Merge。
9. 了解实用的斜杠命令，用于处理上下文、模型、共享，以及可选的云端委托。

为保持工作坊聚焦，你将创建两个 PR：第一个用于星级评分，第二个用于筛选功能及说明更新、技能更新、QA 配置和测试。筛选和质量工作流共用一个会话和分支，以便在探索各项工具时持续基于已有成果。

## 课程

| 课程 | 主题 | 说明 |
| ------ | ----- | ----------- |
| [0. 先决条件][ex0] | 设置 | 创建你的仓库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，然后熟悉环境 |
| [2. 添加星级评分：快速上手][ex2] | 首次改动 | 显示现有评分和空值回退状态，然后合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 从议题规划功能，使用 Autopilot 构建，再在 Interactive 模式中审查 |
| [4. 使用自定义说明指导 Copilot][ex4] | 上下文 | 探索并更新说明，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 探索现有技能，更改报告格式并运行它 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 配置 CLI 中的 MCP，并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求与覆盖 | 创建并选择专业配置文件，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查完整变更、创建 PR，并使用 Agent Merge |
| [9. GitHub Copilot CLI 中的斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享，以及可选的云端委托 |
| [10. 总结与后续步骤][ex10] | 摘要 | 回顾工作流、可复用自定义项和更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、部署基于目录的智能体，并连接到网站 |

## 先决条件

参加本次研讨会前，请确保具备：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 有权创建仓库和 Codespace
- [ ] 对终端或命令行操作有基本了解

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][student-plan] 免费获取 GitHub Copilot。**Copilot Student** 计划包含本工作坊所需的智能体、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> 如果你使用的是 Copilot Business 或 Copilot Enterprise，请确认管理员已为你启用 Copilot CLI。

## 开始

**[从先决条件开始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
