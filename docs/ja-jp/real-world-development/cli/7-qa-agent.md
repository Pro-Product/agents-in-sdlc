---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動 check を実行し、Playwright MCP を使ってブラウザーでのフィルタリング操作を確認しました。次は、明確に定義した QA プロセスを持つカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが instruction、skill、MCP tool と連携する方法を確認する。
- 再利用可能な品質保証 profile を作成して確認する。
- QA agent を選択し、フィルタリング issue に照らして報告内容をレビューする。

## シナリオ

Tailspin Toys は pull request (PR) を作成する前に、要件、コード品質、自動 check、test coverage、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使って QA プロセスを調整し、再利用可能な報告を作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown profile で定義する Copilot の専門的な設定です。profile では、agent の目的、instruction、利用できる tool を指定します。このワークショップでは `.github/agents/qa.agent.md` に QA role を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチーム標準を説明し、`quality-checks` skill は繰り返し実行する check をまとめ、Playwright MCP はブラウザー tool を提供します。QA profile は、これらの機能を要件の評価と結果の報告にどう使うかを Copilot に伝えます。既存の機能を置き換えたり、別の会話を必要としたりするものではありません。

## QA profile を作成する

機能 PR を開く前に、Copilot に再利用可能な QA profile を作成してもらいます。profile では、QA が行う check と、従うべき制約の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定の agent に、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使う前に、Copilot が意図した QA の流れと制約を反映したことを確認します。これにより、検証だけを行うつもりなのに不完全または範囲の広すぎる agent が機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めると agent の表示名がわかりやすくなります。
3. profile の instruction を読み、QA が要件から始め、リポジトリの instruction に従い、`quality-checks` skill を実行し、Playwright MCP を使うことを確認します。
4. QA が根拠とともに結果を報告し、実装コードを変更する前に確認を求め、commit や pull request の作成を行わないことを確認します。
5. これらの役割や制約が不足している場合は、続ける前に既定の agent に修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile を作成したら同じフィルタリングの会話を再開し、QA を選択します。これにより、会話にある issue と計画の決定を利用できます。

1. 次の prompt を入力して agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な agent の一覧に表示されないことがあります。上記の command で有効にできます。

2. QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を開始します。
4. 作業が完了したら、報告を確認します。

## まとめと次のステップ

再利用可能な専門 role を追加し、その作業をレビューしました。このレッスンでは次のことを行いました。

- カスタム エージェントが instruction、skill、MCP tool と連携する方法を確認した。
- 再利用可能な品質保証 profile を作成して確認した。
- QA agent を選択し、フィルタリング issue に照らして報告をレビューした。

実装、skill の更新、QA profile、test、検証報告がそろいました。次は、機能を PR にまとめ、Agent Merge を使います。[機能 PR を作成しましょう][next-lesson]

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
