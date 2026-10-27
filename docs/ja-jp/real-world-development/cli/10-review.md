---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発フローを進めました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントをガイドし、既存の `quality-checks` スキルを調整して、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 要件、テスト カバレッジ、スキルの結果、ブラウザーでの証拠を評価する品質保証 (QA) カスタム エージェントを作成して選択しました。
- 機能全体をレビューし、フィルター PR に Agent Merge を使いました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認しました。

## 作成したもの

ワークショップには、それぞれ `main` から作成する別ブランチの PR が 2 つあります。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価状態を表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、命令を更新して機能に適用し、`quality-checks` の報告形式を調整し、QA プロファイルと関連テストを追加します。

手順を簡潔にするため、フィルターの計画から PR 作成まで同じ会話とブランチを使いました。

## 検証方法の違い

自動テスト、自分でのブラウザー確認、MCP 経由の Copilot によるブラウザー操作でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は要件とテスト カバレッジのレビューにその結果を加えました。

追加するテストは実際のカバレッジ不足を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを承認する前にコードと検証結果を確認し、変更があった場合は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業の結果を左右します。このワークショップでは命令を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを設定しました。チームのニーズに合わせてカスタマイズを再利用し、調整してください。命令は標準を設定し、スキルは繰り返し作業を記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、変更に参加しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に伝えると、出力に大きな違いが生まれます。

## さらに確認する

コア ワークフローは完了しました。Copilot CLI の機能をさらに確認するには、次のコマンドを試せます。

- `/review` で code review エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを使い続けることが、活用方法を身につける一番の方法です。本番コード、個人プロジェクト、作りたいと思っていた小さなアプリなどで試し、学んだことをチームと共有してください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
