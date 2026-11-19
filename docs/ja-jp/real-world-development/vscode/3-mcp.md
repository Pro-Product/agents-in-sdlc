---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request を開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、branch を公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する方法を学ぶ。
- Playwright MCP server を workspace に追加する。
- Copilot にブラウザーを使ってフィルタリング機能を手動テストしてもらう。
- branch を公開し、フィルタリング作業の pull request を作成する。

## Model Context Protocol (MCP) とは

agent mode は editor の外にアクセスできると、さらに多くのことができます。Copilot がそのために使うのが Model Context Protocol (MCP) です。agent が外部の tool やサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、MCP server や VS Code 拡張機能が提供する tool とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部の tool やサービスと通信する方法を提供します。MCP を使うと、AI agent はリアルタイムに外部の tool やサービスとやり取りし、最新情報（resource）にアクセスしたり、ユーザーに代わって操作（tool）を実行したりできます。

MCP server は、AI agent と外部の tool やサービスをつなぎ、通信を管理します。既存の API や NPM package のようなローカル tool など、外部の tool との通信を担当します。各 MCP server は、AI agent が利用できる別の tool と resource のセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API にアクセスできます。AI agent は repository の作成や更新、issue や pull request の管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI agent は Web page への移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまな tool や resource を提供する MCP server がほかにもあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP server はプロジェクト内のほかの依存関係と同じように扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使用し、機密 resource や操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` file で、この VS Code workspace で利用できる MCP server を構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに次の `github` server が構成されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` entry はプロジェクト template に含まれています。そのため、前の演習で Copilot は issue のバックログを読み取れました。これは[リモート GitHub MCP server][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が tool を初めて使う際に GitHub で認証します。

## Playwright MCP server を追加する

次に 2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供します。機能のテストに使用します。

1. `.vscode/mcp.json` で `github` と並べて `playwright` entry を追加し、file を次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. file を保存します。

リモートの `github` server と異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag を指定すると、表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys project では end-to-end test に Playwright を使っているため、必要なブラウザーは通常インストール済みです。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行してください。

## server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。Copilot が server の tool を初めて使うときに server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上に **Start** action が表示されます。選択して server を起動し、構成を確認します。
2. VS Code から server を信頼するか尋ねられたら、構成を確認して信頼するよう選択します。

手動での起動は任意です。スキップすると、次の手順で Copilot が tool を初めて使う際に VS Code が server を起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] は Copilot が操作できる実際のブラウザーを提供します。自分でアプリを操作する代わりに、agent が page を開き、移動し、filter を適用して結果を読み取り、確認内容をまとめます。会話を離れずに機能を確認できる最も速い方法です。

Playwright MCP server はスクリーンショットではなく、page の [accessibility tree][playwright-mcp-server] を使います。そのため agent は、支援技術と同じように、button、link、list item などの構造化されラベル付けされた要素を扱います。簡単な機能確認が、アクセシビリティの簡単な健全性チェックにもなります。

server を接続し、アプリを起動したら、作成したフィルタリング機能を Copilot に試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らし合わせて報告を確認してください。問題があれば、pull request を開く前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` で実行中のアプリが必要です。dev server を停止している場合は、prompt を送る前に再起動します。Copilot が初めて Playwright MCP server を使う際に、ブラウザーのダウンロードが必要になる場合があります。ブラウザーの不足が報告されたら、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## branch を公開して pull request を作成する

機能が動作することを確認したので、チームがレビューできるよう pull request (PR) を作成します。最初に `filtering-vscode` branch を公開します。

1. codespace の **Source Control** panel を開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![変更内容を表示する Source Control panel のスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して branch を repository に push します。

## pull request を作成する

github.com や GitHub command-line interface (CLI) など、pull request を作成する方法はいくつかあります。GitHub Copilot を使って、関連する issue を見つけ、issue に関連付けた PR を作成してもらいます。

1. Copilot Chat panel に移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub tool を使えるよう、agent の dropdown で **Agent** が選択されていることを確認します。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作を Copilot に許可します。GitHub tool を初めて使うときに認証を求められた場合は、画面の案内に従います。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成した link を選択し、pull request をレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する方法を学んだ。
- Playwright MCP server を workspace に追加した。
- リリース前に、ブラウザーからフィルタリング機能を手動でテストした。
- branch を公開し、フィルタリング作業の pull request を作成した。

MCP server を使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティのレビューなど、対象を絞ったタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
