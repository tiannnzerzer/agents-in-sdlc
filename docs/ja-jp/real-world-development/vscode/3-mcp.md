---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動テストさせます。
- ブランチを公開し、フィルター機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部に接続するとさらに便利になります。Model Context Protocol (MCP) は、Copilot が外部ツールやサービスと通信するための標準です。

![コンテキスト、大規模言語モデル (LLM)、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) と agent mode の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを通じて最新情報にアクセスし、ツールを通じて開発者に代わって操作を実行できます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを介してアクセスします。MCP サーバーは既存の API や npm パッケージなどのローカル ツールとの通信を管理し、AI エージェントが利用できるツールとリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。AI エージェントは、リポジトリの作成・更新、issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub はエコシステム内での発見や貢献を促進する [MCP registry](https://github.com/mcp) を運営しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用しているため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能テストに使えます。

1. `.vscode/mcp.json` の `github` の隣に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、ブラウザーを画面に表示せずに実行します。デスクトップ画面を持たない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンド テストに Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**オンデマンド**で起動します。Copilot がサーバーのツールを初めて必要としたときに起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. VS Code でサーバーを信頼するか確認されたら、構成を確認して信頼することを選択し、サーバーを起動します。

手動での起動は省略可能です。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って要約します。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を支援技術と同様に扱います。そのため、簡単な機能確認をアクセシビリティの基本確認にも活用できます。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送り、作成したフィルター機能を操作させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、手順に沿って操作して結果を報告します。報告内容を issue の受け入れ条件と照らし合わせます。動作に問題があれば、プル リクエストを開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> テスト時はアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が初めて Playwright MCP サーバーを使う際、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認したので、チームのレビュー用にプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼します。Copilot は関連 issue を探し、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報取得や操作を実行できるようにします。GitHub ツールを初めて使うときは、GitHub での認証が求められる場合があります。画面の案内に従ってください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が作成したリンクを選択し、プル リクエストをレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Copilot にブラウザーでフィルター機能を手動テストさせました。
- ブランチを公開し、フィルター機能のプル リクエストを作成しました。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどの作業に活用します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
