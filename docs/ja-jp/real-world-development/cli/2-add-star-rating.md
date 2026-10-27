---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードを変更し、転送されたブラウザーで確認して、最初のプル リクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを起動して確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードに表示されるのはタイトル、カテゴリ、パブリッシャー、説明だけです。ウォームアップとして、各カードに既存の評価を表示します。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は Copilot CLI でタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話ではターミナルでチェックアウト中のリポジトリと Git ブランチを使います。保存した会話は後で再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するツールの操作、`/diff` で確認できる変更を見られます。

## 会話を始めて変更を依頼する

機能の実装に向けて新しい会話を始めます。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI をまだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲーム詳細を表示するファイルを見つけて必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューしてください。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲーム カードに表示されることを確認します。
3. テストで両方の状態が扱われていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは例と異なる場合があります。特定の実装にこだわらず、動作を確認してください。

## 変更を確認する

コードを読むだけでなく、Codespaces で転送されたブラウザーを使って、更新した UI を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に次のプロンプトを送り、起動したサーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。変更をプロジェクトにマージするため、プル リクエスト (PR) を作成します。

1. 既定のエージェントに次のプロンプトを送り、変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。PR の作成時に Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択し、PR を開きます。
4. 変更されたファイルとチェックの結果をレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI で最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot の会話を開始しました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを起動して変更を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は[フィルターの issue を起点に Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
