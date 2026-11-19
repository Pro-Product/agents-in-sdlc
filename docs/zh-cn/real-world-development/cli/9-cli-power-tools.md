---
title: "练习 9 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和委派任务到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他一些实用命令。

本练习将带你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派任务到云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。接下来看看一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将任务委派给 [Copilot 云智能体][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理规模较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace。如果尚未打开 Copilot CLI，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统说明、工具、消息和可用空间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能并不明显。

> [!NOTE]
> 上下文窗口接近占满时，Copilot CLI 会自动压缩上下文。若要自行决定压缩时机，请使用 `/compact`。切换到无关任务并希望开启新对话时，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者的偏好也不同。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员一起工作并分享经验，可以帮助大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 在发送给他人或发布之前，检查导出的内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已完成。Copilot CLI 还提供了适用于更大型任务的命令：

- `/fleet` 可以将独立子任务分配给子智能体并行执行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体；它会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建额外的工作树或远程工作。尝试前，请开始一个范围明确的新任务，并按常规工作流审查结果。若要深入了解异步智能体工作，请继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 中的斜杠命令，可以进行配置、共享会话，并查看幕后运行情况。本练习中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派任务到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。现在，[回顾所学内容][next-lesson]，并了解后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
