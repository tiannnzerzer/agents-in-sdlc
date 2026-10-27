---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を行い、プル リクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム命令を追加し、GitHub issue を割り当てて実装を任せます。カスタム エージェントを確認して活用し、agents ダッシュボードからセッションを監視して方向を調整した後、作成されたプル リクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して方向を調整する |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、code review、Copilot CLI の機能が含まれているため、各ハーネスを完了できます。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外の **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
