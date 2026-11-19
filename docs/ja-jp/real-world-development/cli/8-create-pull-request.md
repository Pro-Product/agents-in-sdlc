---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証 (QA) profile、テストが、1 つのブランチに保存されています。まとめてレビューし、pull request を作成しましょう。前回の star-rating pull request (PR) は手動でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能全体の変更と検証の証拠を調べる。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリングのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（失敗した継続的インテグレーション (CI) check の修正、レビュー コメントへの対応、必要に応じた rebase など）に取り組み、リポジトリが許可していれば GitHub の auto-merge を有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードを作成したら、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の部分を自動化しました。次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学んだ。
- 機能全体の変更と検証の証拠を調べた。
- フィルタリングの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意の cloud delegation に役立つ [Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
