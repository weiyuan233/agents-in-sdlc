---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[说明文件][instruction-files]来指导代码生成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档规范如何通过仓库自定义说明及路径范围说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级规范。
- 再次运行相同提示，观察生成的代码如何应用新规范。
- 将说明更新和筛选数据切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有规范。Copilot 具有概率性。本练习的目的是观察更新说明后行为的整体变化趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用暗色模式并具有现代感。
- 应通过 TypeScript 文档注释 (TSDoc) 为代码添加说明。
- 每个文件开头都应添加描述文件用途的注释块。

通过说明文件，可为 Copilot 提供这些实践要求，帮助它按规范完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个仓库或特定文件类型创建说明，提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建说明，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用仓库级说明，以及包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持采用其他标准的说明文件，例如 AGENTS.md 和 CLAUDE.md。详情请查看[自定义说明支持][custom-instructions-support]。

### 管理说明文件的最佳实践

示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或具体任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应配备完善的说明文件。浏览本项目的说明文件时，你会看到针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的文件。
>
> Copilot 也可以协助生成说明文件。不同工作环境的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作环境的相关课程会在需要时说明具体用法。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读仓库自带的说明文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要的项目介绍，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还包含 **GitHub Actions Workflows** 子部分。这些说明适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意 Astro、Drizzle 数据层、测试等说明文件。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于仓库根目录的 glob，确定适用文件。此处匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目关于创建单元测试的具体说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的说明集拆分成可复用的小文件，并提供生成代码时可参考的示例。这里的路径相对于说明文件所在目录，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码内文档。接下来会加入 TSDoc 文档注释和文件头注释规范。

## 为更改创建分支

按照最佳实践，为本次更改创建分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习的模式和模型选择可能不同；相关步骤会在需要时说明。

先在当前说明文件生效时发送提示。之后更新说明并再次发送相同提示，以查看两次结果的差异。

1. 关闭之前练习遗留的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，以 `db` 客户端作为第一个参数，并返回有类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使未明确要求，它也可能添加文档注释。这种情况也没问题；本练习关注的是更新说明后行为的一致性改进。

## 添加新的仓库规范

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来补充仓库编码规范，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了编码规范，但尚未要求代码内文档。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以放在仓库级 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。仓库级文件适合放置刚添加的文档注释等项目规范。

## 再次运行提示并观察变化

说明更新后，再运行相同提示，比较新旧结果，观察项目规范对 Copilot 建议的影响。

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用于真实代码；下一练习会继续使用这些代码。

## 提交说明并推送分支

说明文件与仓库中的其他资源一样，也需要使用源代码管理。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求 (PR) 并关联待办事项中的文档规范议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub；按照提示授权即可。后续练习会介绍此连接的工作方式（模型上下文协议 (MCP)）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 请求你 **Allow** 它执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR，并将本地分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

## 总结与后续步骤

你已探索 Copilot 如何从项目说明文件中获取上下文，然后在 VS Code 中使用 Copilot Chat：

- 探索了项目上下文、编码指南和文档规范如何通过仓库自定义说明及路径范围说明文件提供给 Copilot。
- 使用当前说明和代码生成提示创建了筛选功能的第一个数据切片，并观察 Copilot 生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级规范。
- 再次发送相同提示，观察生成的代码如何应用新规范。
- 将说明更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，使用智能体模式，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
