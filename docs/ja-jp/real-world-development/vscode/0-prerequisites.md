---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリをセットアップする

ラボで使うリポジトリを[テンプレート][template-repository]から作成します。新しいリポジトリには演習に必要なファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリの **Use this template** ボタンを選択し、**Create a new repository** を選択します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催イベントの一環として参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後の手順で使います。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

## Codespace を作成する

続いて、演習用の Codespace を作成します。[GitHub Codespaces][codespaces] は、ブラウザーからコードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールを使える IDE を備えています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ひとまず、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。VS Code が Codespace と同じ dev container を構築します。

## GitHub Copilot Chat と agent mode を使う

VS Code で GitHub Copilot Chat の agent mode を使うには、統合開発環境 (IDE) に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を使用している場合は、通常すでにインストールされています。

> [!TIP]
> 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、Visual Studio Code の Extensions ビューで **GitHub Copilot Chat** を検索し、**Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力し、Enter キーを押します。これで Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の案内に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されていることを確認します。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書くための Codespace の作成を開始しました。

## 次のステップ

Copilot を使って作業を始めましょう。[演習 1 - カスタム命令][next-lesson]に進み、プロジェクトの規約を Copilot に伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
