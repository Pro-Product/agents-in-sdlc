---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue に基づくフィルタリングを実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織で使われている開発フローに沿って、適切なものを適切な方法で作成できるように設計されています。このレッスンは、agent を活用した一般的な開発プロセスをたどるシリーズの最初のレッスンです。まず issue を使って機能を作成し、コードが有効で、機能が期待どおりに動作することを確かめ、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイル種別ごとに異なるブランチや PR を使いますが、ここでは中核となる概念に集中するため、手順を簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能の開発を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がカテゴリーやパブリッシャーでゲームを絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリーの組み合わせなど、コーディング前に合意が必要な詳細もあります。Plan モードで方針を決めたあと、Autopilot に範囲を明確にした実装を許可します。

## 背景

AI コーディング agent を開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始める。
2. 実装するものの計画を立てる。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新しい機能を手動で検証する。
6. pull request (PR) を作成する。
7. コードがレビューされ、継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 正確な手順はチームや organization によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な進め方を守ると、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ審査プロセスを通るようにできます。

## 会話モード

**会話モード**は、agent にどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: agent と一緒に作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent は最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: agent は自律的にコードを書き、テストを実行して、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. star-rating PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue を prompt に追加し、**Session** タブに戻ります。

prompt が `#7`（または同様の番号）で始まることを確認してください。`#` を使うと、GitHub 上の issue や pull request (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能を実装したりタスクを Copilot に任せたりする前に、進め方を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替えて、Copilot に計画を作ってもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。prompt の下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次の prompt を入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリング機能の動作について質問された場合は、自分の希望に基づいて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して、計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って、Copilot に作業を任せます。

Autopilot を使うと、Copilot は完了したと判断するまで課題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の選択肢を選択します。
2. prompt の下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。必ず計画を先にレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、check が完了したと判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードの生成後、ほかのコードと同じように、マージする前にレビューする必要があります。コードをレビューし、サイトを起動して動作を確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. issue と計画中に決めた内容に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して diff 表示を閉じます。
5. プロジェクトの check の出力を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードは問題なさそうですが、実際に動作するでしょうか。以前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を使用可能と通知したら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー フィルター、パブリッシャー フィルター、組み合わせを試します。
4. リセットと検索結果がない場合の動作が、issue と合意内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発 server の停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot 会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動で検証した。

次は、コードが文書化されたプラクティスに従うように、[カスタム instruction を使って][next-lesson]詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
