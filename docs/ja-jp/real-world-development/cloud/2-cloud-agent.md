---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instruction][previous-lesson] |
|:--|

技術的負債に悩んでいない organization は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。agent が作業を終えると、人間の開発者がレビューできる draft PR を作成します。時間のかかる作業を任せることで、開発プロセスを加速し、より重要な作業に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にする重要性を理解する。
- Copilot に issue を割り当てる。

## シナリオ

Tailspin Toys は技術的負債を解消したいと考えています。最初の Web サイトを作成した委託先は、ドキュメントを整備しておらず、ほとんど記載がありません。まず、アプリケーションのすべての export 関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ユーザーがゲームを見続けられるよう、各ゲームの詳細ページに同じカテゴリーのほかのゲームを表示します。完成度の高い実装はまだ必要ありません。UX の受け入れテストで使えるものを用意します。現時点では優先度の高い issue がほかにあるため、この作業は後回しになっています。

どちらも、優先度が下がりやすく、Copilot cloud agent に適したタスクです。agent が非同期に作業している間、開発者はほかの作業を進め、後で戻って作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば [GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると Copilot は進捗を追跡する draft pull request を作成し、環境をセットアップしてタスクに取り組みます。作業中または完了後にセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした instruction の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑さが見つかります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI pair programmer として扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## この repository のカスタム instruction

前の演習ではカスタム instruction と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を割り当てる前に、repository にある instruction file を読み取り専用で確認してください。後で影響を確かめられます。

repository の GitHub web UI、または起動済みの codespace で次の file を開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript 規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter が `**/*.test.ts` file に instruction を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成された pull request で TSDoc doc comment、TypeScript 規約、comment header を確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、適切な状態に保つための特定の環境やセットアップ script が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent は SWE と同じような方法で作業します。

cloud agent は [GitHub Actions][github-actions] を使って環境を準備します。`.github/workflows/copilot-setup-steps.yml` file に設定する[専用の setup workflow][setup-workflow]を作成すると、作業開始前に必要な開発 tool と依存関係をインストールできます。このラボでは、学習の流れを支えるため、事前に構成済みです。単一の Astro app に必要な Node.js、project の依存関係、end-to-end test 用の browser binary、migration と seed 済みの SQLite database が利用できます。

```yaml
name: "Copilot Setup Steps"

# repository の "Actions" タブから setup 手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用の token が与えられます。
    permissions:
      # 依存関係のインストールなどで setup 手順の一部として repository を clone する場合は、`contents: read` 権限が必要です。setup 手順で clone しない場合は、完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / app のセットアップ - Node.js (app 全体で Astro + Drizzle/libSQL を使用)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build と test でデータを使えるよう、ローカル SQLite database を migrate して seed します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub workflow と似ていますが、重要な点が 2 つあります。

- `copilot-setup-steps` という job が 1 つあります。これは Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うと、repository の **Actions** タブから workflow を手動で実行できます。Copilot の実行を待たずに workflow の動作を確認できます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり不足していたりする project は少なくありません。技術的負債を放置すると生産性が低下し、codebase の保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成が得意なため、cloud agent に依頼するのに適した issue です。agent はバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しい browser tab で github.com の repository に移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue dialog を開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当て dialog を開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい pull request (PR) を開きます。一覧に PR が表示されない場合は、少し待って browser window を更新します。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかる場合があります。Copilot は先ほどの workflow を実行して環境を作り、project を分析して、問題への最適な取り組み方を判断しています。

13. todo list と、Copilot が実行するタスクを確認します。
14. pull request の timeline を下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** button を選択します。

  ![Copilot の session view](../../../_images/ex4-view-session.png)

> [!CAUTION]
> indicator の更新を確認するため、window の更新が必要になる場合があります。

16. live session をスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、方針を考えるために一時停止し、コードを作成する過程が表示されます。

この作業には数分かかります。Copilot cloud agent の主な目的は、作業を非同期に進めて開発者が別のタスクに集中できるようにすることです。もう 1 つのタスクも cloud agent に割り当て、その間にアプリケーションへ機能を追加しましょう。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザイン チーム向けに関連ゲームを追加する作業自体は短時間でも、別の作業に使える時間を確保できます。cloud agent に割り当てます。

1. github.com の repository に戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue dialog を開きます。
4. **Blank issue** を選択して blank template を使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当て dialog を開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が issue の最初の comment に表示されます。

![Copilot が issue の作業中であることを目の emoji で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常時監視する必要はありません。完了後にレビューします。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、issue を割り当てて非同期に処理できることを確認しました。cloud agent には技術的負債の解消、新しい機能の作成、framework 間のコード移行などを依頼できます。

次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にする重要性を理解する。
- Copilot に issue を割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使います。[Copilot cloud agent は MCP server も使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent の setup workflow ベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
