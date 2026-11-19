---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、テスト、pull request のレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

あなたは、開発者向けボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しく入社した開発者です。チームのバックログは GitHub issue として登録済みで、すぐに取り組めます。バックログには、フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 開発環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。自分の開発スタイルに合った環境を選んでください。各ワークショップは独自のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、カスタム エージェント、Playwright MCP、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリで、並列エージェント セッションの実行、セッション モードの切り替え、canvas での共同作業、GitHub issue と pull request の管理を行います。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、エージェントが作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP server、カスタム エージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
