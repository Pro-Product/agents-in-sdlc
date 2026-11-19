---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并继续修改……类似步骤还有很多。

Copilot 智能体模式正是为此而设计的。

智能体模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、查找相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，需要让用户按类别和发行商筛选。上一课已经添加了发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解当前功能。

这是一个带有开发者主题的桌游众筹平台，可供用户列出游戏并查看详情。网站是一个 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一课完成后，应已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，选择终端中的 localhost URL 后，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有通道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以查看其功能。主要功能包括：

- **主页**：显示桌游列表、标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。每项练习所需的模式和模型可能不同，相关步骤会注明要求。

网站的初始实现已可正常运行，但还需要添加更多功能。先查看积压工作。根据模板设置项目时，系统已自动创建一组 issue；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以提供良好体验。不过，由于 LLM 的输出具有非确定性，你看到的建议可能与工作坊中的说明不同。这完全正常，也是预期情况。

> [!NOTE]
> 由于 LLM 的输出具有非确定性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示 issue 积压工作：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

Copilot 甚至会根据对用户的帮助程度为项目排序。

## 查看指令文件

让智能体生成代码前，先查看将用于提供上下文的指令文件。这里会使用[用户界面 (UI) 指令][ui-instructions]，了解如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这有助于复用内容、将复杂指令拆分成更易管理的部分，以及提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面并添加筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 智能体模式可以完成这些任务，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择模型 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据积压工作中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行时选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间不断迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、查找相关文件并协调更改。
- 如何在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 - 实现分页

游戏列表不断增长后，需要支持分页。运用本练习中学到的技能，提示 Copilot 为网站添加分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页的实现方式，例如是否允许用户选择页面大小，还是使用固定页面大小。
- 编写提示时，提供 Copilot 所需的指导，帮助它按预期实现功能。
- 可能需要与 GitHub Copilot 迭代交互，要求修改并提供上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
