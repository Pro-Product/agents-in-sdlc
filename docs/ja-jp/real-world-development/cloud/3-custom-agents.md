---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。repository の `.github/agents` フォルダーにある Markdown file で agent を定義し、対象の作業を効果的に進めるための具体的な instruction、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えます。チームの専門知識を再利用可能な agent として形式化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティ agent、安全なコーディング プラクティスに従うセキュリティ agent、一貫した test pattern を保つテスト agent などです。

カスタム エージェントは、project の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML frontmatter を記述し、その後に agent の動作、専門性、instruction を定義する Markdown prompt を続けます。

### カスタム エージェントと agent skill の比較

カスタム エージェントと[agent skill][agent-skills-concept]には、概念上の重なりがあります。どちらも主に Markdown file で定義し、AI に作業方法を伝えます。最も分かりやすい区別は、**カスタム エージェントは作業者、skill はツール**ということです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部として skill（ほかの agent も含む）を調整するように設計されています。このラボでは、アクセシビリティ カスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その作業の一環として、pull request workflow 用の skill や、test を実行・管理する skill などを呼び出すことがあります。

> [!NOTE]
> カスタム エージェントを作る唯一の「正しい」方法はありません。AI のほかの使い方と同様に、環境やシナリオに合う方法を見つけるためにテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、現在の dark theme はテキストと背景色のコントラストが不十分で、読みにくいと指摘されています。このアクセシビリティ上の課題に対応するため、デザイン チームは、ユーザーが切り替えられる高コントラスト モードの実装を求めています。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントがすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認しましょう。

codespace に戻り、アクセシビリティ カスタム エージェントの file を確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタム エージェントに必須です。

3. 続けて、次の内容を説明するセクションを読みます。
   - アクセシブルな Web サイト用コードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## issue を作成して割り当てる

Mission control は、環境内のすべての agent を操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、進行中のセッションに追加のガイダンスを伝えられます。まず、高コントラスト モードの作成を Copilot に依頼します。

1. repository に移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue dialog を開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当て dialog を開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントと Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能な agent として定義し、特定の作業を Copilot がより効果的に進められるようにします。

次のことを確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

Copilot が高コントラスト モードを実装している間に、mission control から[agent セッションを監視して案内します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [organization でカスタム エージェントを使う準備][org-custom-agents]
- [enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
