---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、命令ファイルの編集や cloud agent の成果のレビューに使う [Codespace][codespaces] を起動します。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーを用意します。[テンプレート][template-repository]から新しいリポジトリを作成すると、ラボに必要なファイルがそろいます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリの **Use this template** ボタンを選択し、**Create a new repository** を選択します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催イベントの一環として参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後の手順で使います。

## Codespace を作成する

続いて、ラボの演習に使う Codespace を作成します。[GitHub Codespaces][codespaces] は、ブラウザーからコードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールを使える IDE を備えています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ひとまず、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と作業するための Codespace の作成を開始しました。

## 次のステップ

次は cloud agent が従うカスタム命令を追加します。[演習 1 - カスタム命令][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
