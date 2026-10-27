---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

これまでの演習で、ユーザー エクスペリエンスの改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加のガイダンスを与えたりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同時に追加するのが合理的です。エージェントにハイ コントラストに加えてライト モードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、リポジトリのチェックアウト、作業環境の作成、Model Context Protocol (MCP) サーバーの設定、割り当てられたタスクを完了するための各種手順があります。

> [!NOTE]
> Copilot の作業状況や取り組み方によって、表示される手順は異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここから PR と変更ファイルを見て、さらに進行状況を把握できます。

## cloud agent の作業に追加指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関連するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで、作業中の cloud agent に追加指示を送るダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に追加したことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中の手順を終えた後、新しい依頼を作業に取り入れます。

これまでと同様、完了まで数分かかります。待っている間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、Copilot cloud agent のタスクを監視し、ガイドしました。この mission control では、進行中および完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクの範囲やガイダンスを調整できます。

このレッスンでは、mission control とエージェント ページでタスクを監視し、進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は作成された[プル リクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
