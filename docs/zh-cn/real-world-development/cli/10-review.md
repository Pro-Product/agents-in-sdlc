---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多学习资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续开发工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选功能 issue。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选功能 issue 开始，在 Plan 模式中确定方案、在 Autopilot 模式中构建功能，并在 Interactive 模式中审查结果。
- 使用自定义指令指导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能运行结果和浏览器验证证据。
- 审查完整的筛选功能更改，并批准使用 Agent Merge 合并筛选功能 PR。
- 探索用于管理上下文、模型、共享会话和可选云端委派的斜杠命令。

## 交付的内容

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化工作坊，我们将这些更改合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在创建 PR 前综合检查了这些结果、需求和测试覆盖率。

添加的测试应弥补真实缺口；QA 运行后无需添加新测试也可能是正确结果。授权合并前，请审查代码和证据；更改后应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专业角色，MCP 服务器则用于连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑实施方案，使用 **Interactive** 处理范围明确的更改并保持参与，使用 **Autopilot** 执行范围明确的任务。日常编辑可以选择响应更快的模型；复杂工作则选择能力更强的模型。

上下文依然和基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出结果。

## 进一步探索

核心工作流已经完成。还可以了解一些 Copilot CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种看法。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式！可以用它处理生产代码、个人项目，或一直想做却尚未动手的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以查看 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
