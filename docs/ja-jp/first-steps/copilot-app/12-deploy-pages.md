---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages workflow を設定し、pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot は workflow の準備とデプロイを案内できますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを設定してもらう。
- マージする前に workflow と pull request をレビューする。
- デプロイを進め、公開されたサイトを確認する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、ほかの人が簡単に試せる場所がありません。GitHub Pages を使って単一ファイルのサイトを公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL から利用できるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> pull request の作成では、コードの commit と push が発生する場合があります。この prompt を送る前に、承認設定と対象リポジトリを確認してください。**Approve all** が有効な場合、別途確認が求められるとは限りません。

`space-quiz` の Copilot app セッションで、次の prompt を送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## workflow と pull request をレビューする

1. リポジトリに README がまだない場合に限り、追加されていることを確認します。
2. pull request の変更をレビューします。workflow のビルドとデプロイの手順、トリガー、権限を確認してください。
3. workflow が、マージされていない pull request ではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された diff をレビューします。マージを許可する前に、必須の pull request check が成功するのを待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次の prompt を送ると、レビュー済み pull request のマージ、リポジトリの Pages 設定変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリと pull request を確認してから送信してください。承認設定によっては、これらの操作に別途確認がない場合があります。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えてから、別の prompt として送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを実行できない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは完了と見なさないでください。

## まとめと次のステップ

Space Quiz をライブ サイトとして公開し、エージェントの報告だけに頼らず、デプロイを確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進んでください。

[next-lesson]: ../13-review/
