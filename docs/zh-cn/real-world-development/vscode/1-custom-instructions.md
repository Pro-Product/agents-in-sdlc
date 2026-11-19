---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解背景信息，就应确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[指令文件][instruction-files]指导代码生成。

本练习将带你：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件提供给 Copilot。
- 在当前指令文件下使用提示，生成第一个筛选数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选代码切片提交到 `main`，以便 Copilot 在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能偏离部分标准。Copilot 的输出具有非确定性。本练习旨在观察更新说明后行为的*整体变化趋势*，而不是要求输出逐字符完全一致。

## 指令文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 掌握正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。可以指定编码约定、库以及希望采用的注释类型。可以创建适用于整个存储库的指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含对大多数聊天或 CLI 请求都适用的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：适用于特定任务或文件类型的指令文件。可以为 TypeScript 或 Astro 等特定语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md，详情请参阅[自定义指令支持][custom-instructions-support]。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围，但示例项目展示了一种有代表性的方法。总体来说：

- `copilot-instructions.md` 应专注于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门说明。
- 使用自然语言，保持指导清晰，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应包含完善的指令文件。查看本项目中的文件时，你会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应界面的课程会在相关处介绍。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间阅读本存储库提供的指令文件，其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 说明。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它使用相对于存储库根目录的 glob 来确定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并链接到可参考的示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求为代码添加文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。每项练习所需的模式和模型可能不同，相关步骤会注明要求。

要查看自定义指令的影响，先在当前指令文件下发送提示。之后更新指令并重新发送相同提示，比较结果。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取本次需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数文件和测试。
6. 注意，建议的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回有类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有非确定性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍然是观察更新说明后行为的一致性改进。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但没有代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一课会以此为基础继续开发。

## 提交说明并推送分支

指令文件和其他存储库资源一样，需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已包含：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联积压工作中的文档标准 issue。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具查找 issue、创建 PR 并关闭 issue，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经在工作区中配置了 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，同时描述指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在**现有**说明下生成的结果。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察建议的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，为下一课做好准备。

接下来，在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot - 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
