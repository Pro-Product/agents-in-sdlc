---
title: "演習 1 - カスタム instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] は、そのガイダンスを提供する方法です。Copilot が何をするかだけでなく、どのように進めるかも理解できるようになります。

この演習では、次のことを行います。

- repository のカスタム instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認する。
- `.github/copilot-instructions.md` に repository 全体に適用する新しい標準を追加する。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習では変更前後の prompt を使いません。Copilot cloud agent は GitHub issue を非同期に処理するため、影響をリアルタイムに並べて示すのが難しいためです。後の演習で cloud agent が作成した pull request をレビューするときに、instruction file の効果を確認できます。

## Instruction file

### シナリオ

優れた開発チームと同じように、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- data layer には常に unit test が必要です。
- UI は dark mode で、現代的な印象にする必要があります。
- ドキュメントは TSDoc doc comment としてコードに追加します。
- 各 file の先頭には、file の役割を説明する comment block を追加します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に提供します。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定を与えられます。好みのコーディング規約、ライブラリ、追加したい comment の種類などを指定でき、より関連性の高い提案やコード スニペットを得るのに役立ちます。repository 全体向け、またはタスク レベルのコンテキストとして特定の file 種別向けに instruction を作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は repository への**すべて**のリクエストで Copilot に送信される単一の instruction file です。Copilot に送るほとんどの chat や CLI request に関係するプロジェクト レベルの情報を記載します。たとえば使用する技術スタック、作成中のものの概要、ベスト プラクティス、全体に適用されるガイダンスなどです。
- `.github/instructions/*.instructions.md` file は、特定のタスクや file 種別向けに作成できます。TypeScript や Astro などの言語、UI component や unit test などのタスク向けにガイドラインを提供できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時に repository レベルの instruction と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は [AGENTS.md や CLAUDE.md][custom-instructions-support] など、ほかの標準に基づく instruction file もサポートしています。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法を示す例が含まれています。要点は次のとおりです。

- `copilot-instructions.md` では、作成中のものの説明、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスに集中します。
- `*.instructions.md` file は、file 種別（unit test、Astro component、data layer）または特定のタスク向けの instruction に使います。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

instruction file の作成にも AI の利用にも、唯一の方法はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、充実した instruction file のセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの file が用意されています。
>
> Copilot に instruction file を作成してもらうこともできます。利用方法は surface によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、各 surface のレッスンで説明します。
>
> template や出発点を探していますか。[awesome-copilot][awesome-copilot] repository には instruction file、カスタム エージェントなどのリソースがあります。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instruction file を確認する

この repository に含まれる instruction file を読んでみましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` file が用意されています。editor または GitHub web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` folder を開いて確認します。Astro file、Drizzle data layer、test などの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` field を確認します。この glob は repository root からの相対パスで、instruction が適用される file を決めます。たとえば `**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトで unit test を作成する際の instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて一番下までスクロールします。`unit-tests.instructions.md` のようなほかの instruction file や、プロジェクト内の file へのリンクを確認します。大きな instruction を小さな再利用可能な file に分け、コード生成時に従う例を Copilot に示せます。（記載された path は repository root ではなく instruction file からの相対 path です。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment と file comment header のルールを追加します。

## 新しい標準を追加する

コード変更のために、まず作業用の branch を作成します。cloud agent は issue の対象 branch から instruction を読み込みますが、変更をきれいに commit するために branch を使います。

1. codespace のターミナルで、新しい branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を渡す file です。repository のコーディング標準を文書化して、コード提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、file comment header と TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス用の repository レベル（`.github/copilot-instructions.md`）にも、特定の言語、file 種別、タスク向けの `*.instructions.md` にも作成できます。doc comment ルールのようなプロジェクト全体の標準を置くには、repository レベルの file が適しています。

## instruction を commit、push してマージする

cloud agent は issue の対象 branch から instruction file を読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` から branch を作成するため、instruction の変更を `main` に取り込む必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. branch を push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で issue を割り当てる際に、cloud agent がこの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` を直接使う場合は、branch の作成を省略して `main` に直接 commit できます。実際のプロジェクトでの進め方を体験できるよう、ここでは branch を使っています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に repository 全体の標準を追加しました。cloud agent が今後作成する pull request で、この標準が適用されます。

次は、Copilot cloud agent に[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
