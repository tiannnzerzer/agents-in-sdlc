---
title: "レッスン 8 - 機能のプル リクエスト (PR) を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストは同じブランチに保存されています。これらを一緒にレビューしてプル リクエストを作成します。星評価のプル リクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、命令、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルターと品質ワークフローを同じ会話とブランチにまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認します。
- 機能全体の変更と検証結果を確認します。
- フィルターの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、マージの承認権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プル リクエストをマージ可能にするために必要な作業を自動化します。有効にすると、失敗した継続的インテグレーション (CI) チェックやレビュー コメントへの対応、必要なリベースなど、PR を妨げている問題に Copilot が取り組みます。リポジトリで許可されている場合は、GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択しました。Agent Merge に作業を任せる場合も、まず変更をレビューし、準備ができていると判断してから有効にしてください。

## Agent Merge で PR を管理する

コードをまとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の CI を監視します。すべて成功するとマージを実行します。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可される方法でレビュー済みのマージを手動で完了してください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認しました。
- 機能全体の変更と検証結果を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[GitHub Copilot CLI の便利な slash command][next-lesson]を確認し、コンテキスト、モデル、共有、cloud agent への任意の委任を扱います。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
