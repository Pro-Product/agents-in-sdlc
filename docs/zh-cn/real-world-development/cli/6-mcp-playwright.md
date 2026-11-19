---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot CLI 使用内置功能之外的工具和服务。

本练习将带你：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让智能体操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与 UI 交互。允许 Copilot 像用户一样操作正在开发的网站，可以进一步自动化更改流程，并增强对功能符合预期的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问外部工具和服务，获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具、服务之间的桥梁。每个 MCP 服务器都会提供一组智能体可以访问的工具和资源。

一些常用的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

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

## 让 Copilot 通过 Playwright 体验功能

之前你手动确认了功能是否符合预期。现在通过 Copilot 中的 Playwright MCP 完成同样的操作。

1. 使用以下提示，让 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。但既然知道该使用什么工具，明确指出也有好处！这样能让结果更一致，还能节省一些 token。

2. 观察 Copilot 列出在浏览器中确认功能是否正常的各个步骤。
3. 阅读报告，确认所有功能都符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器，并提供报告。

## 总结和后续步骤

你通过 Copilot CLI 使用 Playwright MCP 在真实浏览器中体验了功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果之前未配置，则添加了 Playwright MCP 服务器。
- 让智能体操作浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
