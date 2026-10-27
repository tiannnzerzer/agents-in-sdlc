---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种主要方式是使用[指令文件][instruction-files]指导代码生成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在现有指令下生成第一个筛选数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 将指令更新和筛选切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不一致。Copilot 的输出具有非确定性。本练习的重点是观察更新指令后行为的*趋势变化*，而非要求输出逐字一致。

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
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
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

## 为更改创建分支

遵循良好实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在终端中打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在适用时说明。

要了解自定义指令的影响，先在现有指令文件的指导下发送提示。之后更新指令文件，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取本次需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它有时可能会在未明确要求的情况下添加文档注释。若出现这种情况也没有问题；本练习的重点是观察更新指令后的*一致性*改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议。

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

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习将继续使用的实际代码。

## 提交指令并推送分支

指令文件和存储库中的其他内容一样，使用相同的源代码管理流程进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础工作更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置好工作区中的 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录。之后的练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，同时说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求 **Allow** 以代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求 **Allow** 以代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，并观察 Copilot 在**现有**指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础工作提交到 `main`，供下一练习接续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
