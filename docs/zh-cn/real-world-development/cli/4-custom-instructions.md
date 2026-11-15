---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "探索仓库说明，添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，就应向 Copilot 提供相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。完成筛选功能后，你将探索 Copilot 使用的说明、添加文档规范，并将其应用于代码。

在本课中，你将：

- 探索仓库说明和按路径应用的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码规范。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释 (TSDoc) 或 JavaScript 文档注释 (JSDoc) 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过说明文件，可为 Copilot 提供这些实践要求，帮助它按规范完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。可以指定编码约定、库以及注释风格。说明既可应用于整个仓库，也可针对特定文件类型提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

先查看入门项目中已有的说明文件，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件中的说明适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹，查看其中的文件；你会看到 Astro、Drizzle 数据层、测试等相关说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置一个 glob，用于确定适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这种方式可将较大的说明集拆分成可复用的小文件，并提供代码示例。

## 更新说明以符合团队规范

现有说明仍有不足。修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载仓库说明。保存更新后，退出并恢复筛选对话，以便在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍在运行，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 使用以下提示要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新的规范应用于功能代码。具体来说，你：

- 探索了仓库说明和按路径应用的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码规范。
- 查看了说明文件对代码的影响。

接下来，自定义并运行可复用的 `quality-checks` 技能，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
