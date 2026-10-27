---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を設定し、ブラウザーからフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードの作成だけでなく、データや外部サービスとの連携、Copilot で利用できる自動化も必要です。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを追加できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- Playwright MCP サーバーが未設定の場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot に開発中のサイトをユーザーのように操作させ、変更が期待どおりに動作するか確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新情報にアクセスし、開発者に代わって操作を実行できます。MCP サーバーがエージェントと外部ツールやサービスの橋渡しをします。各サーバーは、エージェントが利用できるツールやリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プル リクエストを管理する API を提供します。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあり、GitHub はエコシステム内での発見や貢献を促進する [MCP registry][mcp-registry] を運営しています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのようにサイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、続けて <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を閉じます。

## Playwright で機能を確認する

Playwright MCP サーバーを使って、ブラウザーで機能を確認します。

1. 次のプロンプトを Copilot に送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に判断して使うため、明示的に指定する必要はありません。使うサーバーを伝えると、より一貫した結果を得やすくなり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を追っていることを確認します。
3. 報告を読み、すべて期待どおりに動作するか確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認しました。
- Playwright MCP サーバーが未設定の場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は[QA カスタム エージェントを作成し][next-lesson]、スキルとブラウザー ツールを組み合わせます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
