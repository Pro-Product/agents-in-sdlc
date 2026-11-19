---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリング issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージした。
- フィルタリング issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム instruction で agent をガイドし、既存の `quality-checks` skill をカスタマイズして unit test、lint、type check を実行した。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認した。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、coverage、skill の結果、ブラウザーでの確認内容を評価した。
- フィルタリング機能全体をレビューし、Agent Merge による PR のマージを許可した。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認した。

## 提供したもの

このワークショップには、それぞれ更新済みの `main` から作成した別ブランチ上で進める 2 つの PR があります。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示を、ゲーム カードに追加する。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` の報告をカスタマイズし、QA profile と関連テストを追加する。

フィルタリングを計画して PR を作成するまで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## 複数の方法で検証する

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー確認を行いました。`quality-checks` skill は unit test、lint、type check を実行し、新しい形式で結果を報告しました。QA は要件と test coverage のレビューに結果を加えて、PR の前にまとめて確認しました。

追加するテストは実際の不足を埋める必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠をレビューし、変更後は関連する根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストと tool は、作業の質に影響します。このワークショップでは、instruction の更新、skill のカスタマイズ、QA profile の作成、MCP server の構成を行いました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。instruction は標準を設定し、skill は繰り返し実行する作業をまとめ、カスタム エージェントは専門 role を定義し、MCP server は外部 tool に接続します。agent の要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合わせて**モードとモデル**を選びます。作成前の検討には **Plan**、対象を絞った変更に常に関与するには **Interactive**、範囲を明確にしたタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

基盤と同じくらいコンテキストも重要です。何を作りたいのか、なぜ必要なのか、どう進めたいのかを明確に伝えると、出力は大きく変わります。

## さらに確認する

中核となるワークフローは完了しました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` で code review agent に変更を分析してもらう。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どの tool でも、上達する最善の方法は使い続けることです。本番コード、趣味のコード、何年も作りたいと思いながら手をつけていなかった小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学んでください。いつものように、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境で学ぶ場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
