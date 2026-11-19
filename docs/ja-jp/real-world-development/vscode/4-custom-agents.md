---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。repository の `.github/agents` folder 内の Markdown file で agent を定義し、対象の作業を効果的に進めるための具体的な instruction、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えます。チームの専門知識を再利用可能な agent として形式化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティ agent、安全なコーディング プラクティスに従うセキュリティ agent、一貫した test pattern を保つテスト agent などです。

カスタム エージェントは、project の `.github/agents` folder またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML frontmatter を記述し、その後に agent の動作、専門性、instruction を定義する Markdown prompt を続けます。

### カスタム エージェントと agent skill の比較

カスタム エージェントと[agent skill][agent-skills-concept]には、概念上の重なりがあります。どちらも主に Markdown file で定義し、AI に作業方法を伝えます。最も分かりやすい区別は、**カスタム エージェントは作業者、skill はツール**ということです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部として skill（ほかの agent も含む）を調整するように設計されています。このラボでは、アクセシビリティ カスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その作業の一環として、pull request workflow 用や test の実行・管理用の skill などを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作る唯一の「正しい」方法はありません。AI のほかの使い方と同様に、環境やシナリオに合う方法を見つけるためにテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、現在の dark theme はテキストと背景色のコントラストが不十分で、読みにくいと指摘されています。この課題に対応するため、デザイン チームは、ユーザーが切り替えられる高コントラスト モードの実装を求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ agent を使い、高コントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。codespace に戻り、アクセシビリティ用のカスタム エージェントを確認します。

作業を分けるため、Exercise 3 のフィルタリング PR とは別に、`main` から新しい branch を作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタム エージェントに必須です。

3. 続けて、アクセシブルな Web サイト用コードの主な責務、アクセシビリティのベスト プラクティス、HTML/CSS/JavaScript のコード例、よくある落とし穴を説明するセクションを読みます。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタム エージェントが Copilot Chat view 下部の agent dropdown に表示されます。カスタム エージェントを選ぶと、その agent の instruction と tool を使う chat session にできます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選び、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

1. Chat view の agent dropdown で **Agent** を選択します（まだ選択されていない場合）。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. chat view 下部にある agent dropdown（現在の agent を表示します。既定値は **default**）を選択します。
3. 利用可能な agent から **Accessibility agent** を選択します。
4. アクセシビリティ agent に次の prompt を送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が codebase を調べ、変更を提案して project file を編集します。編集内容は file path と diff とともに chat に表示されます。
6. 変更された作業セットを確認できるよう、**Files changed** indicator が更新されます。

> [!NOTE]
> この処理には数分かかります。Copilot は Astro component、CSS、関連 test などの既存 file を変更します。

次の演習では、作業中の内容を確認して、agent に追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能な agent として定義し、特定の作業を Copilot がより効果的に進められるようにします。

次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は、agent の作業を[監視して案内し][next-lesson]、変更を確認しながら同じ session にライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
