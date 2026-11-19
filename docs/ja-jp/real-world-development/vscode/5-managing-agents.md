---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、codebase の調査、変更の提案、file の編集、command の実行を自律的に行います。VS Code の Copilot Chat では、作業中に各 tool call と file の編集を確認できます。diff をその場でレビューし、個々の変更を採用または取り消し、editor を離れずに追加 prompt で作業を調整または拡張できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムに監視する。
- 提案された file の編集を確認して採用する。
- セッション中に agent に指示を追加し、高コントラストに加えてライト モードの切り替えを実装する。

## シナリオ

[Exercise 4][previous-lesson] で使ったアクセシビリティ カスタム エージェントが、高コントラスト モードを実装しています。作業中に、デザイン チームから追加の依頼がありました。既定の theme が暗すぎると感じるユーザーのために、ライト モードの切り替えも追加してほしいとのことです。新しい会話を始める代わりに、進行中の session に指示を追加します。

## Copilot の進捗を監視する

Copilot Chat view では、agent が調査する file、実行する command、提案する編集など、各ステップを確認できます。file の編集は diff と **Keep** / **Undo** control とともに chat 内に表示されます。

1. Exercise 4 で使用した Copilot Chat view に戻ります。
2. agent の作業中に chat をスクロールし、各 tool call を確認します。file の読み込みや編集、実行を求める terminal command が表示されます。
3. agent が file の編集を提案すると、diff が chat 内に表示されます。
    - **Keep** を選択して変更を採用します。
    - **Undo** を選択して変更を取り消し、agent に再試行してもらいます。
4. chat 上部の **Files changed** indicator を開くと、この session で agent が変更した file をすべて確認できます。

> [!NOTE]
> terminal command の実行などで tool の許可を求められると、agent は一時停止して **Allow** prompt を表示します。内容を確認し、許可してもよい command のみ承認してください。

5. agent が高コントラスト モードの実装を終えるまで待ちます。最終メッセージに変更の概要が表示されます。

## agent にライト モードの追加を依頼する

高コントラスト モードの実装後、同じ会話でライト モードの切り替えも追加します。会話を続けることで、agent は前の作業で変更した component、採用した style の方法、設定した local storage の pattern を引き継げます。

1. Copilot Chat の入力欄に、次の追加 prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent は既存の変更を調べ、ライト モードの追加を提案します。
3. 高コントラストのときと同じように、新しい編集を確認し、それぞれ **Keep** または **Undo** を選択します。
4. agent が意図しない方向に進んだ場合（たとえば、希望しない方法で toggle UI をリファクタリングする場合）は、chat 入力欄の横にある **Stop** button で停止し、追加 prompt で説明します。

> [!TIP]
> 進行中の session に指示を追加すると、最初からやり直すより短時間で調整できます。追加 prompt で、変更（「CSS を簡潔にし、selector の重複ではなく CSS variable を使う」）、軌道修正（「toggle を 3 つにせず、ライト モードを既存の設定 panel に追加する」）、範囲の拡張（「両方の toggle に keyboard shortcut も追加する」）を依頼できます。

## 作業内容をレビューする

commit する前に、agent が変更した内容をすべて確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更された file の一覧を確認します。Astro component、style、関連 test などが更新されています。
3. 変更 file をいくつか開き、diff を確認します。カスタム エージェントのアクセシビリティ パターン（ARIA attribute、keyboard navigation、semantic HTML、local storage による保存）が反映されていることを確認します。
4. 問題がなければ、Source Control panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode で、進行中の session を監視して案内しました。各 file の編集を確認して採用または取り消し、コンテキストを保ったまま追加 prompt で作業を拡張しました。

このレッスンでは、次のことを行いました。

- agent の作業をリアルタイムで監視した。
- 個々の file 編集を採用または取り消した。
- 追加 prompt で進行中の session に指示を加えた。

次のレッスンでは、アクセシビリティの作業を pull request として公開し、このラボを通じてローカルのカスタム エージェントが行った変更をレビューします。[次のレッスン][next-lesson]に進んでください。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode を使う][agent-mode]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
