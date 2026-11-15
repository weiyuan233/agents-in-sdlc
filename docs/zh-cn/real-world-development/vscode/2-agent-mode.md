---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试。开发者往往需要找到所有相关文件、完成更改、运行测试、调试、找出遗漏文件并再次修改。Copilot 智能体模式可以更自主地在 IDE 中工作：它会探索项目结构、执行必要更新、运行测试，并自动修复发现的问题。接下来使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，支持快速反馈和交互。cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协同修改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、用户界面 (UI) 和测试工作。

## 运行 Tailspin Toys 网站

修改代码前，先体验网站，了解当前功能。

该网站是一个带开发者主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle 对象关系映射 (ORM) 直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口 (API) 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准文档对象模型 (DOM) API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一练习完成后，当前应位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 网站服务器的专用隧道。

### 浏览网站

网站运行后，可以浏览其功能：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习的模式和模型选择可能不同；相关步骤会在需要时说明。

网站的初始实现已可运行，但还需要添加新功能。先查看待办事项。使用模板创建项目时会自动创建一组议题；让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便尽量保持建议的一致性。不过，由于大语言模型 (LLM) 具有概率性，你收到的建议可能与示例不同，这是正常且符合预期的。

> [!NOTE]
> Copilot 可能使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看 GitHub 仓库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 检查生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度排列优先级。

## 审查说明文件

开始生成代码前，先审查将用于提供上下文的说明文件。此处将使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件，路径相对于说明文件所在位置。这样可以复用说明、拆分复杂指导，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更改：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以协助完成这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 说明文件。

   ![在 Copilot Chat 中选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，要求 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 智能体模式正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应按钮；无需在工作过程中提前选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中请求确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，或看到测试暂时失败。这是正常的；Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完成任务后的 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

你已使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。具体来说，你了解了如何：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让智能体模式探索项目、识别相关文件并协同修改。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 Playwright MCP 服务器测试功能，并为其创建拉取请求。

### 附加探索练习：实现分页

随着游戏列表不断增长，需要启用分页。使用本练习学到的技能，提示 Copilot 为网站添加分页。需要考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 确定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时提供 Copilot 完成所需的指导。
- 可能需要通过提供上下文并要求更改来迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
