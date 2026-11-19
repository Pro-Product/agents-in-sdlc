---
title: "レッスン 9 - GitHub Copilot CLI の便利な slash command"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使える slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、多くの slash command が用意されています。これらの command を使うと、Copilot の高度な機能、内部の状態、追加の構成オプションを確認できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利な機能をいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で、利用可能なモデルを確認する。
- `/share` で、セッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意の command を確認する。

## シナリオ

CLI の中核的なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の作業委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに近づくことがあります。Copilot CLI は必要に応じて会話を自動的に compact します。slash command を使えば、コンテキストの確認や手動での compact もできます。

1. Codespace に戻り、まだ起動していない場合は repository root から Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、system instruction、tool、message、空き領域に分割されたコンテキストを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比べます。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的に compact します。実行するタイミングを選びたい場合は `/compact` を使います。関係のないタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに異なる強みがあり、開発者によって好みも異なります。Copilot CLI では、利用するモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学びを共有すると、全員が AI tool をより上手に使えるようになります。`/share` command では、セッションを Markdown または HTML file にエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにおける `/share` の選択肢を確認します。
2. セッションを共有する場合は、たとえばローカルの Markdown file にエクスポートする `/share file` など、必要に合った出力先を選びます。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには prompt、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのない repository や会話の内容を公開しないでください。

## 任意: 作業を並列化または委任する

このワークショップの中核部分は完了しました。Copilot CLI には、大きなタスクに使える command もあります。

- `/fleet` は、独立したサブタスクを subagent に割り当て、並行して実行できます。
- `/worktree` は、別タスク用の独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、pull request を作成する場合があります。

これらの command は追加の worktree やリモート作業を作成する可能性があるため、任意の項目です。試す場合は新しく範囲の明確なタスクから始め、通常のワークフローで結果をレビューしてください。非同期 agent 作業について詳しく知るには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更したり、セッションを共有したり、内部の状態を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` で、利用可能なモデルを確認した。
- `/share` で、セッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意の command を確認した。

Copilot CLI には、さらに多くの slash command と機能があります。次は、[学んだことを振り返り][next-lesson]、学習を続ける方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
