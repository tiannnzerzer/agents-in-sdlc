---
title: "レッスン 7 - 品質保証 (QA) エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた品質保証 (QA) カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。これらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター issue の調査結果をレビューします。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使うと、QA の手順を調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、目的、命令、利用可能なツールを Markdown プロファイルで定義する、Copilot の専門エージェントです。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義します。

各カスタマイズには異なる役割があります。リポジトリの命令はチーム標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらのカスタマイズの代わりになるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、チェック内容と制約を定義した QA プロファイルを作成します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送信します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の手順と制約が記載されていることを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は任意ですが、エージェントの表示名として便利です。
3. 要件から始め、リポジトリの命令に従い、`quality-checks` スキルと Playwright MCP を使うよう指示されていることを確認します。
4. 根拠を示した報告を行い、実装コードを変更する前に確認し、変更をコミットしたり PR を開いたりしないことを確認します。
5. 要件や制約が欠けている場合は、先に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、フィルターの会話を再開し、QA エージェントを選びます。

1. 次のプロンプトでエージェントを選択します。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業が終わったら、報告を確認します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター issue の調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証結果がそろいました。次は[機能の PR を作成して Agent Merge を使い][next-lesson]、変更をまとめてレビューします。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
