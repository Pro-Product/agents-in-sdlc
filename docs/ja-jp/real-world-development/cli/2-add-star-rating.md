---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示してもらい、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を開始する。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- 転送されたブラウザーでアプリを実行し、変更を確認する。
- 最初の pull request を作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリー、パブリッシャー、説明だけが表示されています。準備運動として、各カードに既存の評価を表示します。最初のセッションにぴったりな、小さく独立した変更です。

## 会話の構成

**会話**では、Copilot CLI と一緒にタスクに取り組みます。Copilot app と異なり、通常の CLI 会話では専用の worktree を作るのではなく、ターミナルでチェックアウトされているリポジトリと Git ブランチを使用します。保存した会話を後から再開できます。ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、prompt とエージェントの応答、ファイルを調査・編集するエージェントの tool 操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装に向けて、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次の prompt で変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細表示に使われるファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## diff をレビューする

小さな変更であっても、AI が生成した変更はマージ前にレビューしてください。Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. ゲーム カードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行した check の結果をレビューし、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して diff 画面を閉じます。

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は決定論的ではなく確率的に動作するため、コードは毎回異なる場合があります。特定の実装と一致することではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を使用可能と通知したら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されていることを確認します。
4. Copilot に戻り、起動した server の停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能が完成しました。新しいコードをプロジェクトにマージする pull request (PR) を作成します。

1. 既定の agent に変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に、Copilot CLI は既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら URL を選択します。
4. 変更されたファイルと check をレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。次のことを行いました。

- 機能ブランチで Copilot との会話を開始した。
- エージェントにゲーム カードの小さな変更を依頼した。
- `/diff` で変更をレビューした。
- 転送されたブラウザーでアプリを実行し、星評価を確認した。
- 最初の pull request を作成してマージした。

次は、[フィルタリング issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
