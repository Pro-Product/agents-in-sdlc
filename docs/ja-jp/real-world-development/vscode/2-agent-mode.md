---
title: "演習 2 - GitHub Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数の file の変更やテスト実行などが必要です。開発者は、必要な file を見つけ、変更し、テストを実行し、問題をデバッグし、更新し忘れた file を探して、さらに変更を加えます。作業は次々と増えていきます。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同様に既存の project 構造を調査し、必要な更新を行い、テストなどの task を実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作するため、すばやいフィードバックと対話が可能です。cloud agent はチームの一員のように非同期で働き、issue と pull request を通じてやり取りする AI pair programmer です。

この演習では、次のことを学びます。

- Copilot Agent Mode が project を調べ、関連 file を見つけて複数の変更を調整する方法。
- GitHub Copilot Agent Mode が UI と data layer にまたがる機能を実装する方法。
- codebase にマージする前に、Copilot Agent Mode が生成した変更と test をレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習では publishers helper を追加しました。残りの data layer、UI、test の作業を Copilot Agent Mode で完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング platform です。ゲームを一覧表示し、その詳細を確認できます。単一の Astro app であり、build 時に page を静的 HTML として render します。page は frontmatter 内で Drizzle ORM を通じてローカル SQLite database を直接 query します。別の backend API や client-side UI framework はありません。再利用可能な data-access helper は `src/lib/` にあり、操作性は標準 DOM API を使った小さな範囲の Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` task を使うと、単一の Astro dev server を起動できます。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えて `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押し、新しい terminal window を開きます。
3. フィルタリング作業用の branch を作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次の command で Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server が起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開く browser window の dialog が表示された場合は、**x** を選択して閉じます。

5. terminal に表示された Astro server の address `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> codespace の terminal で localhost URL の link を選ぶと、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web server をホストしている codespace への private tunnel です。

### Web サイトを確認する

Web サイトが起動したら機能を確認します。主な機能は次のとおりです。

- **Home Page**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細 page が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なり、必要な箇所で説明します。

Web サイトの初期実装は動作しますが、機能を追加して改善します。まずバックログを確認しましょう。template から project をセットアップすると issue のバックログが自動作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat view の agent dropdown で **Agent** を選択します。**Agent** は file をまたいだ変更を自律的に計画・実装し、terminal command を実行して tool を呼び出します。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくすることを意図しています。ただし LLM は確率的に動作するため、説明と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP command を使う場合がありますが、タスクは完了できるはずです。

3. 次の prompt を Copilot に送信し、issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考えた項目に基づいて優先順位も付けます。

## instruction file を確認する

agent にコードを生成させる前に、Copilot にコンテキストを渡す instruction file を確認します。Web サイトへの機能追加方法が記載された [user interface (UI)][ui-instructions] file を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のガイダンスを確認します。次の内容が含まれています。
   - architecture の概要。
   - component の設計、testability、アクセシビリティの原則。
   - 次の file 種別に関する instruction への link。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> instruction file からほかの instruction file や project 内の file を参照できます。path は instruction file の場所からの相対 path です。これにより、再利用や複雑な instruction の分割、例や template の提示が容易になります。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- data layer (`src/lib/`) にフィルタリング ロジックを追加または調整する。
- フィルタリングの動作に関する test を追加または更新する。
- ゲーム一覧 page にフィルタリング UI を追加する。

さらに、codebase にマージする前に test を実行し、成功させる必要があります。Copilot Agent Mode にこれらを依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を指定します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat view の agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 関連するバックログ issue に基づいて機能を実装するよう、次の prompt を Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot が project を調べ、機能に関連する data-layer helper、UI、test を見つける様子を確認します。その後 file の変更と test の実行が始まります。

   ![Copilot が project file を調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は project の調査、file の変更、test の実行などを行います。タスクや codebase の複雑さによっては数分かかります。作業中、code editor に **Keep** と **Undo** button が表示されることがあります。完了するとすべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択して test を実行します。

   ![Copilot Chat pane で test の実行確認を求める dialog のスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、test が失敗したりする場合があります。問題ありません。Copilot はタスクを完了して error を検出しなくなるまで、コード生成と test を繰り返します。

   ![Copilot Agent Mode の Chat session 完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを起動している browser に戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat window で **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加する方法を確認しました。

- GitHub Copilot Agent Mode で UI と data layer にまたがる機能を実装できる。
- Copilot Agent Mode が project を調べ、関連 file を見つけて複数の変更を調整できる。
- codebase にマージする前に、Copilot Agent Mode が生成した変更と test をレビューする。

次は、[Playwright MCP server で機能をテスト][next-lesson]し、pull request を作成します。

### 追加の演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加してもらいます。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベスト プラクティスに従う。
- page size をユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを prompt に含める。
- Copilot に変更やコンテキストを伝えながら、何度かやり取りする場合がある。これは通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: 各モードの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
