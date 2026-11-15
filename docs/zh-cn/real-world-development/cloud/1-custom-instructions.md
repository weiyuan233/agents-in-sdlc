---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档规范如何通过仓库自定义说明及路径范围说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不通过前后提示来演示效果。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示差异。后续审查 cloud agent 创建的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TypeScript 文档注释 (TSDoc) 为代码添加说明。
- 每个文件开头都应添加描述文件用途的注释块。

使用说明文件可确保 Copilot 了解这些实践要求并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个仓库或特定文件类型创建说明，提供任务级上下文。

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
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成可复用的小文件，并提供生成代码时可参考的示例。这里的路径相对于说明文件所在目录，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码内文档。接下来会加入 TSDoc 文档注释和文件头注释规范。

## 添加新的仓库规范

接下来要编辑一次：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释头。之后的练习中，cloud agent 处理议题时会遵循这项规范。

编辑前先创建分支。cloud agent 会从议题目标分支读取说明，但仍应以清晰方式提交更改。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来补充仓库编码规范，以改善代码建议。

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了编码规范，但尚未要求代码内文档。
4. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以放在仓库级 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。仓库级文件适合放置刚添加的文档注释等项目规范。

## 提交、推送并合并说明更新

cloud agent 会从议题目标分支读取说明。下一练习中，Copilot 会从 `main` 分支创建分支，因此说明更改必须先合并到 `main`，cloud agent 才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上打开从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后练习中分配议题时，cloud agent 就会读取这些说明。

> [!TIP]
> 如果希望在整个工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的处理方式。

## 总结与后续步骤

你已探索 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的仓库级规范。后续工作坊中，你可以在 cloud agent 创建的拉取请求中看到该规范的影响。

接下来，将第一个议题分配给 Copilot cloud agent。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
