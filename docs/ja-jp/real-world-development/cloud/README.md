---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、pull request を作成します。その間、別の作業を続けられます。

この一連の演習では、まず cloud agent が従うカスタム instruction を追加し、GitHub issue を割り当てて実装を任せます。作成された pull request をレビューし、カスタム エージェントを使って作業の進め方を調整し、agents dashboard からセッションの進捗を監視・案内したあと、pull request をレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム instruction][ex1] | コンテキスト | cloud agent が従うカスタム instruction を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して案内する |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは organization で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していませんか。条件を満たす学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できます。Copilot Business または Enterprise では、管理者が organization に対して有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP server を使う方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
