---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と一緒にユーザー体験の改善や機能追加に取り組みました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を作成し、高コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

何度も確認したように、生成 AI を導入してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体に review を有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期に、監視なしで作業するため、安全を保つためのセキュリティ制約が設定されています。

- Copilot は repository への読み取りアクセスのみを持ち、書き込みは作業に使う branch **のみ**に行えます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- MCP server を含め、[外部 resource へのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初の pull request (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準 PR interface を使います。

> [!NOTE]
> PR を確認する際に、GitHub Copilot が firewall によってブロックされたという warning が表示される場合があります。Copilot は外部 MCP server への接続など、外部 resource へのアクセスが既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent の firewall をカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の repository に戻ります。
2. **Pull Requests** を選択して pull request の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、pull request に **[WIP]** flag が表示されます。その場合は作業完了まで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment やそのほかのドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。Exercise 2 の最初に確認したカスタム instruction file による TSDoc doc comment、TypeScript 規約、comment header に注目します。
6. 更新をレビューし問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す indicator が表示されていることを確認します。
8. workflow が承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の check section に workflow が追加されます。問題がなければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

pull request 上で Copilot とやり取りすることもできます。チーム メンバーと同じように、PR やコード内の inline comment で Copilot に言及します。Copilot は comment を受け取り、対応するための新しい session を開始します。結果は確率的に変わるため、依頼内容を一律に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コード file の先頭に、file の役割を説明する短い comment header を追加する。
- TypeScript と Astro file に TSDoc doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. Copilot に **@copilot** を付けて comment を追加し、生成されたドキュメントの変更を依頼します。上記の例や、codebase に追加したいドキュメントについての依頼を使います。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新のために新しい session を開始します。
3. **Back to pull request** を選択すると pull request に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しい commit が表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで反復できます。問題がなければ PR を draft から ready に変更し、main branch にマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 更新を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. workflow の承認待ちを示す indicator が表示されていることを確認します。
7. workflow が承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request の check section に workflow が追加されます。問題がなければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこの branch に切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動して terminal を開き、Copilot が作成した branch 名（例: **copilot/fix-8**）を `<branch-name>` に置き換えて、次の command を実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように cloud agent に更新を依頼して反復できます。たとえば表示する関連ゲームの数を調整したり、comment header や TSDoc doc comment を追加したりできます（この task はカスタム instruction を更新する**前**に割り当てました）。前と同じく、**Conversation** タブで comment を追加すると Copilot が確認して新しい session を開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントが実装した機能をレビューします。この PR には、Exercise 3 で依頼した高コントラスト モードと、Exercise 4 の mission control で追加したライト モードが含まれているはずです。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合は、pull request に **[WIP]** flag が表示されます。その場合は完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 実装をレビューします。特に、次の点を確認します。
   - mode を切り替える toggle UI component。
   - ユーザー設定を保存する local storage の使い方。
   - 高コントラスト モードとライト モードの CSS または style の変更。
   - アクセシビリティ attribute（ARIA label、keyboard navigation など）。
   - mode の切り替えを管理する JavaScript/TypeScript code。
6. 更新を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す indicator が表示されていることを確認します。
8. workflow が承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の check section に workflow が追加されます。問題がなければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace で branch を切り替えてアクセシビリティ機能を手動でテストできます。Codespace に移動して terminal を開き、Copilot が作成した branch 名を `<branch-name>` に置き換えて次の command を実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    続いてアプリケーションを起動し、browser で高コントラストとライト モードの toggle をテストします。期待どおりに動作し、page reload 後も設定が保存されることを確認してください。

アクセシビリティ カスタム エージェントにより、Copilot はアクセシビリティのベスト プラクティスに沿って機能を実装できました。アクセシビリティ上の懸念や改善点があれば、前の PR と同様に comment で **@copilot** に更新を依頼できます。

## 任意の演習 - さらに作業を委任する

cloud agent は、実際のバックログ task を渡して別の作業に集中できるときに効果を発揮します。習慣にするため、repository に issue をいくつか登録して Copilot に割り当てます。たとえば次のような task があります。

- ゲーム詳細 page に backer interest form を作成する。
- ゲーム一覧 page にページネーションを実装する。
- data-access helper に入力検証と error handling を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **repository に用意されたカスタム instruction file を確認**し、cloud agent の出力への影響を確認した。
- **issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認した。
- **アクセシビリティ用カスタム エージェントを作成して使用**し、高コントラストとライト モードの切り替えを追加した。
- **Copilot agents page を mission control として使い**、作業中のアクセシビリティ session を監視して指示を追加した。
- **cloud agent の pull request をレビューして反復**し、`@copilot` に変更を依頼して workflow を承認した。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内の Copilot Chat agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot mode、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、terminal からレビュー済み変更を 2 つリリースします。

自分の repository では、次のことを試してください。

- リファクタリングや test coverage の issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent の setup workflow ベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall の構成][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
