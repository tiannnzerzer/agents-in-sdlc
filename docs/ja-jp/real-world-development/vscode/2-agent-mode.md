---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連ファイルを探して変更し、テストを実行し、問題をデバッグし、更新漏れのファイルを見つけて再度変更します。作業は続きます。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で作業し、すばやいフィードバックとやりとりを可能にします。cloud agent はチーム メンバーのように非同期で作業し、issue とプル リクエストを通じて連携します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode がユーザー インターフェイス (UI) とデータ レイヤーにわたって機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。次は Copilot Agent Mode で、残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認して現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を確認できます。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle オブジェクト リレーショナル マッピング (ORM) を使ってローカル SQLite データベースを直接クエリします。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側ユーザー インターフェイス (UI) フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、操作性は標準の Document Object Model (DOM) API を使うスコープ限定の Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えて `main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Codespace 内の Web サーバーを公開するプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、主な機能を確認します。

- **Home Page**: タイトル、画像、説明を含むボード ゲーム一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、手順内で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいところです。テンプレートからプロジェクトを作成したときに issue のバックログも自動作成されています。GitHub Copilot にバックログの内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数ファイルにわたる変更の計画と実装、ターミナル コマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。結果を比較しやすくすることを意図していますが、大規模言語モデル (LLM) は確率的に動作するため、提案は例と異なる場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送り、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドの実行を許可するため、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える順に issue を優先したことを確認します。

## 命令ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認します。[ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する方法が記載されています。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。アーキテクチャの概要、コンポーネント設計・テスト容易性・アクセシビリティの原則、ファイル形式ごとの命令へのリンクなどがあります。リンク先には Astro、Tailwind CSS、Drizzle の命令ファイルが含まれます。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用し、大きなガイダンスを小さな単位に分け、参考例やひな形を示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルター ロジックを追加または調整する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 命令ファイルを追加します。

   ![UI 命令ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ レイヤー、UI、テストのファイルを見つけて更新し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調査している様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースによっては数分かかります。処理中にコード エディターに **Keep** と **Undo** が表示される場合があります。作業が完了すると、変更ごとに **Keep** または **Undo** を選択できます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot が作業している間、処理が止まったり、一時的にテストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、次のことを確認しました。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を行う方法。
- GitHub Copilot Agent Mode がユーザー インターフェイスとデータ レイヤーにわたって機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してください。プロンプトを作るときは、次の点を検討します。

- 命令ファイルの使用など、既存のベスト プラクティスに従う。
- ページ サイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更やコンテキストを伝えて反復する。これは Copilot を使う通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
