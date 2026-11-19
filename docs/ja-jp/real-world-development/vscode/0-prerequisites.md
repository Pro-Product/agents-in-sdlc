---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys repository のコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリをセットアップする

演習で使うコードのコピーを作成するため、[template][template-repository] から repository のインスタンスを作成します。新しいインスタンスにはラボに必要な file がすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. ラボ repository のページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる organization に新しい repository を作成できます。

    ![repository template の設定を入力する](../../../_images/ex0-repository-settings.png)

4. ラボで後から参照できるよう、作成した repository のパス（**organization-or-user-name/repository-name**）を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> template から repository を作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

## codespace を作成する

次に、ラボの演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、tool に対応する、すべての機能を備えた IDE を利用できます。

1. 作成した repository に移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot のほかの機能を確認しておきましょう。次の演習から使い始めます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。しばらくの間は、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行するように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンした repository を VS Code で開き、prompt が表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使用していれば、通常はすでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または Visual Studio Code の Extensions view を開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールしたら、GitHub アカウントで認証して有効にする必要がある場合があります。

1. codespace に戻ります。
2. editor の右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力し、Enter を押します。これで Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の案内に従って認証します。

    ![Copilot Chat の認証 prompt の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

おめでとうございます。ラボ用 repository のコピーを作成しました。また、コードを書き始めるときに使用する codespace の作成も開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム instruction][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
