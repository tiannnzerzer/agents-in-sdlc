---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善编写文档，事实上，代码完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏，也就是相同类别的其他游戏，以便访问者继续浏览。团队目前不需要精致的实现，只需要能用于用户体验（UX）验收测试的功能。这个请求目前受阻，但其他议题的优先级更高。

这两项任务都可能很快被降级优先级，因此很适合分配给 Copilot 云智能体。智能体可以异步处理，开发者则继续完成其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台处理任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，使用它并不意味着可以省略思考。即使看似简单的操作，深入分析后也会发现其复杂性。因此，[为 Copilot 云智能体明确任务范围][cloud-agent-best-practices]非常重要。把它当作 AI 结对程序员：分阶段工作、不断学习和试验，并根据结果调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给云智能体前，先只读查看存储库中已有的指令文件，稍后便能观察其影响。

在 GitHub Web 界面中打开以下文件；如果已有运行中的 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件，之后的练习会再次说明。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目处于良好状态。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行工作。

云智能体使用 [GitHub Actions][github-actions]作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便访问所需的开发工具和依赖项。实验室已预先配置此工作流，以确保练习顺利进行。它会为单一 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro 和 Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试能够使用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件看起来与其他 GitHub 工作流相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这很适合测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

虽然大家都了解文档的重要性，但大多数项目的文档要么过时，要么缺失。这类技术债务经常得不到处理，降低工作效率，也让维护代码库或帮助新开发者加入团队变得更加困难。Copilot 擅长生成文档，因此很适合将这项任务分配给 Copilot 云智能体。它会在后台生成所需文档，你将在后续练习中回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定合适的处理方法。

13. 查看列表及 Copilot 将要执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码和理解项目状态、暂停思考并制定方案，以及创建代码。

这可能需要几分钟。Copilot 云智能体的一项主要优势是异步执行任务，让开发者能够专注于其他工作。现在利用这一点，再分配一项任务给云智能体，然后将注意力转向为应用添加功能。

## 在游戏详情页添加相关游戏部分

前面提到，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组任务。为设计团队添加相关游戏部分可能不会花很长时间，但也可以利用这段时间处理其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入 `Show related games on the game details page`。
6. 在 **Description** 中输入：

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

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

片刻后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需一直监控；等它完成后再审查即可。现在继续了解如何创建和使用自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本练习介绍了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
