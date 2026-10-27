---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者的工作流程往往包括：查找所有相关文件、进行修改、运行测试、调试、找出遗漏的文件，再次更新……这样的工作还会不断继续。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式旨在让 IDE 中的 Copilot 更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，可快速反馈并与你交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调多处更改。
- 使用 GitHub Copilot 智能体模式跨用户界面（UI）和数据层实现新功能。
- 在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个开发者主题棋盘游戏众筹平台，允许用户浏览游戏列表并查看游戏详情。整个网站是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围较小的 Astro `<script>` 和标准文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单一 Astro 开发服务器。按照以下步骤，在 GitHub Codespace 中运行该任务：

1. 返回 codespace。上一练习结束后，你应已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
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

   Astro 开发服务器就绪后，会显示包含 URL 的提示，类似于：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将其转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到当前网站服务器的私有隧道。

### 探索网站

网站运行后，可以探索它的功能。主要功能包括：

- **主页**：显示棋盘游戏列表及其标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在适用时说明。

网站的初始实现已经可以正常使用，但还可以添加新功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；现在让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，确保体验良好。但由于大型语言模型（LLM）的输出具有概率性，你可能会看到与说明不同的建议。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令列出所有议题。
5. 查看生成的议题列表。

留意 Copilot 还根据它认为对用户最有用的程度，为各项任务安排了优先级。

## 查看指令文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中关于添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这种方式便于复用内容、将复杂指令拆分为较小且易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项不同的更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并代码前必须运行测试并确保通过。Copilot 智能体模式可以协助完成这些工作。现在开始添加功能。

1. 可以在当前 Copilot 对话中继续，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会有 **Keep** 或 **Undo** 按钮，因此无需在工作过程中选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现停顿，甚至有测试暂时失败。这没有问题。Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。我们了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 智能体模式探索项目、识别相关文件并协调多处更改。
- 在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 拓展练习——实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供足够指导，让 Copilot 按照你的设想实现功能。
- 可能需要通过提供更多上下文和要求更改来迭代 Copilot。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
