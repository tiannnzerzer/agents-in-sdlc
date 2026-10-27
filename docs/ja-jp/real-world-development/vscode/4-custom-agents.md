---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、目的に合わせた命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにまとめると、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` フォルダーに置く Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整して作業します。このラボでは、アクセシビリティ用カスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その作業の一環として、プル リクエスト ワークフロー用やテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいと感じるユーザーがいます。デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

この演習では、アクセシビリティ用カスタム エージェントを確認し、Copilot Chat でそのエージェントを使ってハイ コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot に与えるガイダンスを把握します。

1. Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタム エージェントに必須です。

4. 続くセクションを読み、アクセシブルな Web サイト用コードを生成する際の主な役割、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、その命令とツールに沿った会話を始められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、手順内で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。選択されていない場合は選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェント名が表示され、既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が追加されると、エージェントが変更した作業ファイルを確認できる **Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかる場合があります。Copilot はリポジトリを実際に変更し、Astro コンポーネント、CSS、関連テストなどの既存ファイルを更新します。

この作業の進行状況は、次の演習で確認して方向を調整します。

## まとめと次のステップ

[カスタム エージェント][custom-agents]は、特定のタスクや分野に特化した GitHub Copilot のアシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をガイドできます。

この演習では、次のことを行いました。

- 既存のアクセシビリティ用カスタム エージェントを確認しました。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装しました。

次は[エージェントの作業を監視して方向を調整します][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
