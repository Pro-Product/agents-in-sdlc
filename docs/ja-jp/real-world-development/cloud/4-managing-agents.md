---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの 2 つの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents page][agents-page]など、さまざまなツールで管理できます。この mission control では、open pull request (PR) があるすべての agent task を確認できます。実行された操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- agents page を使い、cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加し、機能を依頼する。

## シナリオ

高コントラスト モードを作成するよう agent に割り当てたあと、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているので、ライト モードも合わせて実装するのが合理的です。高コントラスト モードに加えてライト モードも作成するよう、agent の作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents page](https://github.com/copilot/agents) に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
    - codebase のドキュメントを更新する。
    - ゲーム変更用 API を生成する。
    - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような処理があります。
    - repository からコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP server をセットアップする。
    - 割り当てられたタスクを完了するために、さまざまな操作を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。ここから PR と変更ファイルを確認し、作業を監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、高コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラスト モードの追加に関するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. 数分間セッションを確認し、セットアップが完了して作業が始まったことを示すメッセージが表示されるまで待ちます。
3. **Steer active session while Copilot is working** dialog に、次の prompt を追加します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents page の cloud agent task と「Steer active session while Copilot is working」dialog を示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業に組み込むことを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼を作業の流れに追加します。

この処理には数分かかります。休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、案内する中心的な場所である Copilot agents page を確認しました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業を確認し、進行中のタスクに指示を追加して範囲やガイダンスを変更できます。

次のことを確認しました。

- mission control と agents page で cloud agent のタスクを監視した。
- 実行中のセッションに指示を追加し、機能を依頼した。

Copilot がアクセシビリティ機能を完成させる間に、Copilot が作成した pull request を[反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
