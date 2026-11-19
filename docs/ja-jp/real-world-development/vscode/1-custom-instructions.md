---
title: "演習 1 - カスタム instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを使えるようにします。Copilot にコンテキストを渡す方法はいくつかあります。特に重要なのが [instruction file][instruction-files] です。コード生成の指針を提供できます。

この演習では、次のことを行います。

- repository のカスタム instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認する。
- 現在の instruction を使い、フィルタリング用の最初のデータ部分（publishers helper）を作成する。
- `.github/copilot-instructions.md` に repository 全体の標準を追加する。
- 同じ prompt を再実行し、生成コードに新しい標準が反映される様子を確認する。
- instruction の更新とフィルタリング用の変更を `main` に commit し、次の演習で新しいガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、instruction の更新後に振る舞いの傾向がどう変わるかを確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

優れた開発チームと同じように、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- data layer には常に unit test が必要です。
- UI は dark mode で、現代的な印象にする必要があります。
- ドキュメントは TSDoc doc comment としてコードに追加します。
- 各 file の先頭に、file の役割を説明する comment block を追加します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に提供します。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定を与えられます。コーディング規約、ライブラリ、追加したい comment の種類などを指定でき、関連性の高い提案やコード スニペットを得るのに役立ちます。repository 全体向け、または特定の file 種別向けの instruction を作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は repository への**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、作成中のものの概要、ベスト プラクティスなど、ほとんどの chat や CLI request に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` file は、特定のタスクや file 種別向けに作成できます。TypeScript や Astro などの言語、UI component や unit test などのタスク向けにガイドラインを提供できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時に repository レベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は [AGENTS.md や CLAUDE.md][custom-instructions-support] など、ほかの標準を使った instruction file もサポートしています。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプル プロジェクトの例は、代表的な方法を示しています。要点は次のとおりです。

- `copilot-instructions.md` では、作成中のものの説明、プロジェクト構造、全体的なコーディング標準などのプロジェクト レベルのガイダンスに集中します。
- `*.instructions.md` file は、file 種別（unit test、Astro component、data layer）または特定のタスク向けの具体的な instruction に使います。
- 自然な言葉を使い、明確なガイダンスと望ましいコード・望ましくないコードの例を示します。

instruction file の作り方にも AI の使い方にも、唯一の方法はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべての project に、充実した instruction file を用意することをお勧めします。この project には、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file を作成してもらうこともできます。利用方法は surface によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、各 surface のレッスンで説明します。
>
> template や出発点を探していますか。[awesome-copilot][awesome-copilot] repository には instruction file、カスタム エージェントなどのリソースがあります。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## この project のカスタム instruction file を確認する

この repository に含まれている instruction file を読んでみましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` file が用意されています。editor または GitHub web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. project の簡潔な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。これらは Copilot とのやり取りに適用されます。
3. `.github/instructions` folder を開きます。Astro file、Drizzle data layer、test などの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は repository root からの相対 glob を指定します。この例では `**/*.test.ts` に一致する TypeScript test file が対象です。
5. この project で unit test を作成するための instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて一番下までスクロールします。`unit-tests.instructions.md` などの instruction file や、project 内の file への link を確認します。大きな instruction を小さな再利用可能な file に分け、コード生成で参考にする例を示せます。（path は repository root ではなく instruction file からの相対 path です。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment と file comment header のルールを追加します。

## 変更用 branch を作成する

変更用の branch を作成し、ベスト プラクティスに沿って進めます。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しい terminal を開きます。
3. branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction 更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なり、必要な箇所で説明します。

カスタム instruction の影響を確認するため、まずは現在の instruction file を使って prompt を送信します。その後 file を更新し、同じ prompt をもう一度送信して違いを見ます。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習の editor tab を閉じます。
2. helper の配置先を知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat view の agent dropdown で **Agent** を選択し、file を変更できるようにします。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot は project を調べ、helper file と test などにコードの変更を適用します。
6. 提案された helper が型付き関数で、最初の引数に `db` client を受け取り、型付き publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の data layer 規約によるものです。
7. 提案コードに TSDoc doc comment と file-level comment header が**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction 更新後に一貫性が高まることが重要です。

## 新しい repository 標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。repository の標準を記載し、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、file comment header と TSDoc doc comment を Copilot に追加させます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス用の repository レベル（`.github/copilot-instructions.md`）にも、特定の言語、file 種別、タスク向けの `*.instructions.md` にも作成できます。doc comment ルールのようなプロジェクト全体の標準には、repository レベルの file が適しています。

## prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して buffer をクリアします。
2. Copilot が適切な file に注目できるよう、`src/lib/publishers.ts` をもう一度選択します。
3. 先ほどと**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のような comment block があることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しい project 標準を適用させ、次の演習で利用する実際のコードに反映しました。

## instruction を commit して branch を push する

instruction file も repository 内のほかの資産と同じように source control で管理します。変更を commit して branch を push します。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押し、新しい terminal window を開きます。
2. 次の command を実行し、instruction と helper の変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper 基盤の変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (たとえば test) が表示される場合は、それらの file も追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch を repository に push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

branch を push したら、バックログのドキュメント標準に関する issue と関連付けて pull request を作成します。手動で行う代わりに、すでに project に接続済みの GitHub tool を Copilot に使わせます。issue を見つけ、issue を閉じる PR を作成してマージします。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。project template には GitHub tool が設定済みなので、追加設定は不要です。初回の tool 使用時に VS Code から GitHub へのサインインを求められた場合は、案内に従って許可します。後の演習では、この接続のしくみである Model Context Protocol について学びます。

1. codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat view を開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を探し、現在の branch から PR を作成して、instruction の更新と今後のフィルタリング作業に使う publishers helper の基盤の両方を説明するよう依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探し、PR を作成します。
5. Copilot が代わりに GitHub 操作を実行するための **Allow** を求めたら、command を確認して適切に **Allow** を選択します。
6. PR が作成されたら、次の prompt で Copilot に PR のマージと branch の `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作と shell command の **Allow** を求められたら、内容を確認して適切に選択します。

GitHub Copilot の支援で pull request を作成してマージできました。

## まとめと次のステップ

この project の instruction file から Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の instruction を使ってコード生成 prompt を送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` に repository 全体の標準を追加した。
- 同じ prompt を再実行し、提案コードに新しい標準が適用される様子を確認した。
- instruction の更新と helper 基盤を `main` に commit し、次の演習で利用できるようにした。

次は、Copilot が codebase に機能を追加する際に [agent mode で instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
