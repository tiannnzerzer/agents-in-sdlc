---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスごとの命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムの比較が難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には開発上のガイドラインがあります。

- データ レイヤーには必ず単体テストを用意します。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な見た目にします。
- コードには TypeScript Documentation (TSDoc) のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。命名規則、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体または特定のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に渡される単一のファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に適用します。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスクに関するガイダンスを記載できます。

> [!NOTE]
> 統合開発環境 (IDE) では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の命令ファイルにも対応しています。詳細は[カスタム命令のサポート情報][custom-instructions-support]を参照してください。

### 命令ファイル管理のベスト プラクティス

命令ファイルの作成方法をすべて説明することはこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な構成例があります。基本的には次の点を意識します。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル形式や、特定のタスクに関する命令を記載します。
- 自然な言葉で明確に書き、良いコードと避けるべきコードの例を示します。

命令ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトに合わせて試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルの生成を依頼することもできます。各環境で方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。詳しい手順は各環境のレッスンで説明します。
>
> テンプレートやひな形を探すには、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの命令ファイルや既存ファイルへのリンクを確認します。大きな命令を再利用可能な小さなファイルに分け、コード生成の参考例に誘導できます。ここに記載されたパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントの要件はありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

この演習で行う編集は、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーをコードに含めるよう、プロジェクト全体の規則を追加することです。以降の演習で cloud agent が issue に取り組むとき、この規則を参照します。

編集の前に作業ブランチを作成します。cloud agent は issue の対象ブランチにある命令を読み込みますが、ここでは変更を個別にコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト全体に関する情報を Copilot に伝えるファイルです。コーディング標準を追加して、生成コードのガイダンスを改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存のコーディング標準に、コード内ドキュメントの規則がまだないことを確認します。
3. ファイル ヘッダーと TSDoc コメントを追加するよう、既存の標準の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、リポジトリ全体の標準は `.github/copilot-instructions.md` に、言語やファイル形式、タスクに固有の命令は `*.instructions.md` に記載できます。今回追加したドキュメント コメントの規則は、リポジトリ全体に適用するため前者に記載します。

## 命令の変更をコミット、プッシュ、マージする

cloud agent は issue の対象ブランチから作業を開始します。次の演習で issue を Copilot に割り当てると、`main` からブランチが作成されます。命令を読み込ませるには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプル リクエストを作成し、マージします。次の演習で割り当てる issue に取り組むとき、cloud agent がこの命令を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトでの作業方法に近づけるためのものです。

## まとめと次のステップ

この演習では、次のことを行いました。

- リポジトリのカスタム命令とパスごとの命令ファイルから、Copilot にコンテキストが伝わる仕組みを確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。

このハーネスの後半で、cloud agent が作成するプル リクエストにその標準が反映されているかを確認します。

次は[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令作成のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
