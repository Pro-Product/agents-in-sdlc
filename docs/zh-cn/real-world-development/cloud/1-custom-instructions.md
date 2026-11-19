---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 理解不仅要做**什么**，还要**如何**完成。

本练习将带你：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用提示进行前后对比。Copilot 云智能体会异步处理 GitHub issue，因此难以实时展示对比效果。后续审查云智能体创建的拉取请求时，你会看到指令文件的影响。

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

花些时间阅读本存储库中的指令文件，其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 说明。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它使用相对于存储库根目录的 glob 来确定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并链接到可参考的示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求为代码添加文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来要编辑的是唯一内容：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的方式记录代码。之后的练习中，云智能体会读取此规则。

编辑前，先创建分支（云智能体会读取 issue 目标分支中的说明，但仍建议单独提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会读取 issue 目标分支中的说明。下一课将 issue 分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。之后的练习中，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过分支操作，直接提交到 `main`。这里使用分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，云智能体生成的拉取请求会体现该标准。

接下来，[将第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot - 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
