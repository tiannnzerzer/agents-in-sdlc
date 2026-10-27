---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間がレビューできるドラフトのプル リクエスト (PR) を作成します。手間のかかる作業を委任して開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを開発した委託先が、十分なドキュメントを残しませんでした。まず、アプリケーションのエクスポート関数すべてに TypeScript Documentation (TSDoc) のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにします。現時点では完成度の高い実装ではなく、ユーザー エクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度の高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクで、Copilot cloud agent に適しています。非同期で処理を任せて別の作業に取り組み、後から戻って成果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境をセットアップしてタスクに取りかかります。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、Copilot が PR で開発者に通知します。

## 範囲を明確に定めた命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして段階的に進め、学び、試し、適宜調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

これまでの演習ではカスタム命令と、その働きを確認しました。カスタム命令の演習を終えていれば `.github/copilot-instructions.md` の効果を確認しています。まだの場合は、この機会に見てみましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認しておくと、後で影響を見つけやすくなります。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに命令を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。生成された PR に TSDoc コメント、TypeScript 規約、ファイル ヘッダーが反映されているか確認してください。これらの命令ファイルが影響しています。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、適切な状態を保つために環境の準備とセットアップ スクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を利用できるように環境をカスタマイズできます。このワークショップでは、学習のための環境を事前に設定しています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # ここで必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ中に依存関係をインストールするなど、リポジトリをクローンする場合は contents: read が必要です。ここでクローンしない場合は、セットアップ完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一 Astro アプリ用にローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが古い、または存在しないプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に任せるのに適した issue です。後の演習で、生成されたドキュメントをレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい PR を開きます。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. todo リストと、Copilot が実行するタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断し、コードを作成する過程が表示されます。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、非同期にタスクを実行し、開発者が別の作業に集中できることです。この利点を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間を別の作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに Copilot が作業中であることを示す 👀 が表示されます。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、完了まで積極的に監視する必要はありません。作業が終わったらレビューします。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を Copilot に割り当てて非同期に処理できます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
