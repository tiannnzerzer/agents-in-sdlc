---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。コード生成のガイダンスを伝える方法の 1 つが[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスごとの命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 既存の命令を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 命令の更新とフィルター機能の基盤を `main` にコミットし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、命令を更新した後の動作の**傾向**を確認します。生成結果が文字単位で同じになることを期待しないでください。

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

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に渡される単一のファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
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
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[ユーザー インターフェイス (UI) の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルの生成を依頼することもできます。各環境で方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。詳しい手順は各環境のレッスンで説明します。
>
> テンプレートやひな形を探すには、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトに含まれる命令ファイルを確認してから変更し、効果を比較します。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、このリポジトリで Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが適用対象の glob を定義していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存ファイルへのリンクを確認します。命令を再利用可能な小さなファイルに分け、コード生成の参考例に誘導できます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルには十分なガイダンスがありますが、新しく生成する TypeScript の TSDoc コメントについては規則がありません。まず変更前にプロンプトを送り、結果を確認します。その後、命令を更新して同じプロンプトを再送信します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

### 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、手順内で説明します。

現在の命令ファイルを使ったプロンプトを最初に送り、その後命令を更新して、同じプロンプトを再送信します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. Copilot がヘルパーの場所を把握できるように `src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。ヘルパー ファイルとテストが変更される場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 生成コードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくてもドキュメント コメントが生成されることがあります。その場合も問題ありません。命令を更新して出力の一貫性を高めることがこの演習の目的です。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` はプロジェクト全体に関する情報を Copilot に伝えるファイルです。コーディング標準を追加して、生成コードのガイダンスを改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準に、コード内ドキュメントの規則がまだないことを確認します。
3. ファイル ヘッダーと TSDoc コメントを追加するよう、既存の標準の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の標準は `.github/copilot-instructions.md` に、言語やファイル形式、タスクに固有の命令は `*.instructions.md` に記載できます。今回追加したドキュメント コメントの規則は、リポジトリ全体に適用するため前者に記載します。

## プロンプトを再実行して変化を確認する

命令を追加した後、同じプロンプトを Copilot Chat にもう一度送信します。

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が対象ファイルを把握できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot をガイドして新しいプロジェクト標準を適用し、次の演習で使うコードに反映しました。

## 命令ファイルをコミットしてブランチをプッシュする

命令ファイルもほかのリポジトリ ファイルと同様に、バージョン管理の手順に沿ってコミットします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、命令の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 命令の更新と、ヘルパーの基盤作成で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示される場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、命令標準の issue に関連付けたプル リクエストを作成します。GitHub ツールがプロジェクトに接続されているので、Copilot に issue を探し、PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールが設定済みのため、追加設定は不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の案内に従って認証してください。この接続の仕組み (Model Context Protocol) は後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 次のプロンプトを送り、命令ファイルの更新と、フィルター作業に備えた publishers ヘルパーの基盤作成を説明し、現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻して最新状態にします。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行を許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージしました。

## まとめと次のステップ

命令ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- リポジトリのカスタム命令とパスごとの命令ファイルが Copilot にコンテキストを伝える仕組みを確認しました。
- 既存の命令を使ってフィルター機能の最初のデータ部分 (publishers ヘルパー) を生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認しました。
- 命令の更新とフィルター機能の基盤を `main` にコミットし、次の演習で新しいガイダンスを使えるようにしました。

次は[agent mode][next-lesson]を使い、Copilot にコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令作成のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
