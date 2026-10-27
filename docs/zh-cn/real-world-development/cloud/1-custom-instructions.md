---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作环境不同，这里不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。之后审查云智能体创建的拉取请求时，你会看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代感。
- 应通过 TypeScript 文档注释（TSDoc）为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库，以及希望添加的注释类型。可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个单一指令文件发送给 Copilot。文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建此类文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成内容时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不深入讲解如何编写指令文件，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明什么样的代码符合或不符合要求。

指令文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践可以找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。查看本项目文件时，你会看到其中包含针对多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同工作环境的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应工作环境的课程会在适用时介绍这些操作。
>
> 如果需要模板或起点，可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob 模式，决定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目中关于创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型指令集拆分成较小且可重复使用的文件，也能为 Copilot 提供可参考的示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求添加代码文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要进行本练习中唯一的编辑：添加一项项目级规则，要求通过 TSDoc 文档注释和文件注释头记录代码文档。稍后分配议题时，云智能体会读取这项规则。

1. 先创建一个分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应接近第 27 行）。此部分记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级文件（`.github/copilot-instructions.md`）中设置全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令文件。下一课将把议题分配给 Copilot，而 Copilot 会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后，云智能体处理分配给它的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来的练习中，可以在云智能体生成的拉取请求里看到这项标准的应用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 响应的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
