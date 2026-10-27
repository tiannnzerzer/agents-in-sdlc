---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI モード、カスタマイズ、MCP ツール、プル リクエスト自動化を学びながら、Tailspin Toys の変更を 2 つ実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、ひと続きの Tailspin Toys ワークフローに沿って作業します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして概要を確認します。
2. 星評価の変更を行い、ブラウザーでレビューして、最初のプル リクエスト (PR) を手動でマージします。
3. フィルター issue を起点に Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの命令を更新して、フィルター機能に適用します。
5. 既存の `quality-checks` スキルを調整し、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター動作を確認します。
7. 品質保証 (QA) カスタム エージェントを作成して使い、要件、カバレッジ、検証結果を確認します。
8. 機能全体をレビューし、Agent Merge でフィルター PR を処理します。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認します。

ワークショップでは PR を 2 つ作成します。星評価の PR と、命令の更新、スキルの更新、QA プロファイル、テストを含むフィルターの PR です。フィルターと品質ワークフローでは同じ会話とブランチを使い、各ツールの学習内容を積み重ねます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証する |
| [2. 星評価を追加する][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム命令で Copilot をガイドする][ex4] | コンテキスト | 命令を確認して更新し、フィルター機能に適用する |
| [5. quality-checks スキルを調整して使う][ex5] | 繰り返し可能なチェック | 既存スキルを確認し、報告形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を設定し、フィルター動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選び、最終検証結果を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
