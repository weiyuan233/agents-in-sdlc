---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或长期留在待办事项中的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码和添加功能等任务。完成后，它会创建草稿拉取请求 (PR)，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更重要的工作。

你将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初创建网站的承包商没有为代码编写文档。首先，团队希望为应用程序中所有导出的函数添加 TypeScript 文档注释 (TSDoc)。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页上显示相关游戏（即同类别的其他游戏），用于用户体验探索和验收测试。目前不要求实现得很精致；该功能目前属于阻碍项，但还有更高优先级的议题。

这类任务很容易被延后，适合交给 Copilot cloud agent 异步处理。开发者可继续处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 PR 以跟踪进度、设置工作环境并开始处理任务。你可以在任务进行中或完成后查看 Copilot 会话。方案准备好供你审查时，Copilot 会在 PR 中提及你。

## 明确界定说明和任务范围

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，也可能包含复杂细节，因此要谨慎界定交给 Copilot cloud agent 的任务范围。将其视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本仓库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已完成自定义说明练习，你已经看到 `.github/copilot-instructions.md` 的作用；否则，可以在分配任务前查看仓库中的说明文件，以便稍后观察它们的影响。

在 GitHub Web 界面或 Codespace 中，以只读方式打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给 cloud agent。观察生成的 PR 是否包含 TSDoc 文档注释、TypeScript 约定和文件头注释；这些内容来自说明文件。后续练习审查 PR 时还会再次检查。

## 为 Copilot cloud agent 设置开发环境

编写代码通常需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也是如此，它会像软件工程师一样处理任务。

cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。可通过创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，并在 Copilot 开始工作前运行，以安装所需开发工具和依赖项。实验已预先配置此工作流，以确保流程顺利并提供学习机会。它会为单个 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在仓库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆仓库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆仓库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从仓库的 **Actions** 选项卡手动运行工作流，以验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

虽然每个人都知道文档的重要性，但大多数项目的信息要么过时，要么完全缺失。这类技术债务常被忽视，会降低效率并增加维护代码库或引入新开发者的难度。Copilot 擅长生成文档，因此很适合将此议题分配给 cloud agent；它会在后台生成所需文档，后续练习中再审查结果。

1. 在新浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![在议题页面将任务分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中创建待办列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 查看待办列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并决定方案，以及创建代码。

这通常需要几分钟。Copilot cloud agent 的一个主要目标是异步执行任务，让你可以处理其他工作。接下来利用这一点，将另一个任务分配给 Copilot，同时着手为应用添加功能。

## 在游戏详情页添加相关游戏区域

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏区域不需要很长时间，委托给 Copilot 仍能腾出时间处理其他工作。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![在议题页面将任务分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论上看到 👀，表示 Copilot 已开始处理。

![议题评论中的眼睛表情符号表示 Copilot 正在处理任务](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。cloud agent 的工作方式类似于软件工程师，因此你不需要持续监控；等待任务完成后再审查即可。接下来创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、新功能或框架迁移等任务。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作。接下来创建并使用自定义智能体。[Copilot cloud agent 也可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并能读取前面练习中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
