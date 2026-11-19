---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP tool、pull request 自動化を学びながら、Tailspin Toys の変更を 2 つ実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できます。codebase を調査してコードを生成し、command を実行して外部 tool に接続できます。すべて command line から行えるため、グラフィカル editor に切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. GitHub Codespaces で project を準備し、Copilot CLI をインストールして基本を確認します。
2. 星評価の小さな変更を加え、browser でレビューし、最初の pull request (PR) を手動でマージします。
3. フィルタリング issue から始め、Plan mode で方針を定義し、Autopilot mode で構築してから、Interactive mode でレビューします。
4. repository の instruction を更新し、フィルタリング作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、project の check を実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、browser でフィルタリングを確認します。
7. 品質保証 (QA) カスタム エージェントを作成し、要件、coverage、検証の証拠をレビューします。
8. フィルタリングの変更全体をレビューし、その PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使える slash command を確認します。

ワークショップを簡潔にするため、作成する PR は 2 つです。星評価の PR に続き、instruction、skill、QA profile、test を更新したフィルタリングの PR を作ります。フィルタリングと品質のワークフローでは、1 つの会話と branch を使い、tool を確認しながら作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | repository と Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して Interactive でレビューする |
| [4. カスタム instruction で Copilot をガイドする][ex4] | コンテキスト | instruction を確認して更新し、フィルタリングに適用する |
| [5. quality-checks skill のカスタマイズと使用][ex5] | 繰り返し実行できる check | 既存の skill を調べ、報告形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | browser での確認 | CLI で MCP を構成し、フィルタリングの動作を確認する |
| [7. QA agent を作成して使う][ex7] | 要件と coverage | 専門 profile を作成して選び、最終検証の証拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の便利な slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認する |
| [任意: Foundry を組み込む][foundry] | ホスト型 agent | model を準備し、catalog を参照する agent を deploy して Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub account
- [ ] repository と Codespace を作成する権限
- [ ] terminal または command line の基本的な知識

> [!TIP]
> 有料プランを利用していませんか。条件を満たす学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれます。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
