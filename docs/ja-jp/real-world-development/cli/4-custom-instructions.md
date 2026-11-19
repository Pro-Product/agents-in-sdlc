---
title: "レッスン 4 - カスタム instruction で Copilot をガイドする"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要があるなら、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を作成したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file が agent にどのように渡るかを確認する。
- コーディング標準に従うよう instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

優れた開発チームと同じように、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` の export 関数は、`db` 引数を注入する場合はその説明も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述する。
- 再利用可能な Astro component は `Props` の契約を文書化し、関連コードが変わった際にはコメントも最新の状態に保つ。
- 既存の書式設定と lint のガイダンスを維持する。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## Instruction file

Custom instruction を使うと、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定を与えられます。好みのコーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体に適用する instruction file と、特定のファイル種別に適用するものを作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクのガイドラインを提供します。

> [!NOTE]
> その他の instruction 形式やサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム instruction のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム instruction file を確認する

サンプル プロジェクトには、すぐに始められるよう instruction file が含まれています。変更による影響を確認する前に、既存の内容を調べましょう。

1. Codespace に戻ります。
2. Codespaces editor で（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、内容を確認します。Astro file、Drizzle data layer、test などの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction が適用されるファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内の既存ファイルへのリンクを確認します。大きな instruction を小さな再利用可能なファイルに分け、コード生成の参考例を Copilot に示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、追加のルールが必要です。中核となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc comment が追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存したら、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま Copilot に新しいガイダンスを読み込ませます。

1. フィルタリングの会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ root から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のように依頼し、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript file を確認します。追加された TSDoc comment がコードを正しく説明していることを確認してください。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file が agent にどのように渡るかを確認した。
- コーディング標準に従うよう instruction file を更新した。
- instruction file がコードに与える影響を確認した。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint と test を一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム instruction を追加する][instruction-files]
- [カスタム instruction のサポート][custom-instructions-support]
- [カスタム instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
