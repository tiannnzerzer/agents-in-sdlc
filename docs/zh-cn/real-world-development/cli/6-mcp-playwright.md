---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是编写代码。我们还要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。模型上下文协议（MCP）服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

在本课中，你将：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体驱动浏览器并检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面（UI）更新还需要实际操作 UI。你希望 Copilot 能像用户一样使用正在开发的网站，从而自动检查更改并增强对功能符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时获取最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表一组 AI 智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助开发者发现资源并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请检查其源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

你之前已手动确认功能是否正常。现在使用 Copilot 中可用的 Playwright MCP 服务器，重复检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，因此不一定需要明确要求。不过，既然你知道要用哪个服务器，指出来可以帮助 Copilot 更稳定地选择工具，也能节省一些 token。

2. 观察 Copilot 在浏览器中逐步执行操作，确认功能是否正常。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中探索了功能。本课中，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了它。
- 让智能体驱动浏览器并检查筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
