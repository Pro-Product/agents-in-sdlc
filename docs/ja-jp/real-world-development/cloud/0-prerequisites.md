---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。[codespace][codespaces] を作成し、instruction file を編集したり cloud agent が作成した内容をレビューしたりします。

## ラボ用リポジトリをセットアップする

演習で使うコードのコピーを作成するには、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれています。演習中はこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる organization に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボで後から参照できるよう、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

## codespace を作成する

続いて、ラボの演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot のほかの機能を確認しておきましょう。次の演習から使い始めます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。しばらくの間は、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行するように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成しました。また、Copilot cloud agent と一緒に作業するための codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム instruction を追加します。[演習 1 - カスタム instruction][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
