---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱ったり、Copilot が使える自動化を追加したりする必要があります。ここで MCP server が役立ちます。MCP server を使うと、CLI に組み込まれている機能を超えて、Copilot にツールやサービスを追加できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解する。
- まだ利用できない場合は Playwright MCP server を追加する。
- agent にブラウザーを操作させ、フィルタリング機能を試す。

## シナリオ

unit test と end-to-end test は重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot にユーザーと同じように Web サイトを使わせることで、変更が期待どおりに動作する確信を高め、更新をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部のツールやサービスと通信するための方法です。MCP を使うと、AI agent は外部ツールやサービスとリアルタイムにやり取りできます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらの tool や resource には、AI agent と外部ツールやサービスをつなぐ MCP server を通じてアクセスします。各 MCP server は、AI agent が利用できる別の tool と resource のセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub repository、issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP server が利用できます。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI で構成されている場合は、セットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次の command を入力し、MCP server の追加 dialog を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** dialog に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP dialog を閉じます。

## Playwright を使って Copilot に機能を試してもらう

これまでは機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP server を使って同じ確認を行います。

1. 次の prompt を使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Playwright MCP server を使うよう Copilot に指示しなくても、自動的に選択します。ただし、使用する tool がわかっている場合は明示してもかまいません。結果が安定し、token の節約にも役立ちます。

2. Copilot がブラウザー上で機能を確認するために実行する手順を見ます。
3. 報告を読み、すべてが期待どおりに動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作し、作業後に server を停止して結果を報告します。

## まとめと次のステップ

おめでとうございます。Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学んだ。
- まだ利用できない場合に Playwright MCP server を追加した。
- agent にブラウザーを操作させ、フィルタリング機能を試した。

次は、skill と browser tool を組み合わせた [QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
