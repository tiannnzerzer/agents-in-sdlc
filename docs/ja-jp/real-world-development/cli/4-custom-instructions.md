---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクの進め方や Copilot が知っておくべき背景情報を、Copilot が参照できるようにします。[命令ファイル][instruction-files]には、必要なコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、Copilot が参照した命令を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパスごとの命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準を徹底するために命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発上のガイドラインがあります。コメントではコードの繰り返しではなく意図や分かりにくい判断を説明すること、`db/` と `src/lib/` のエクスポート関数には TypeScript Documentation (TSDoc) / JavaScript Documentation (JSDoc) で目的、パラメーター、戻り値を記述すること、再利用可能な Astro コンポーネントの `Props` 契約を文書化すること、関連コードが変わったときにコメントも最新の状態に保つこと、既存の書式設定と lint のガイダンスを維持することなどです。命令ファイルを使い、Copilot がこうした要件に沿って作業できるようにします。

## 命令ファイル

カスタム命令は、コーディング スタイルや要件を Copilot に伝えます。リポジトリ全体または特定のファイルに適用できます。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に渡されます。技術スタックや開発中の内容、全体に適用するガイダンスを記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関するガイダンスを記載します。

> [!NOTE]
> 他の命令ファイル形式とサポート状況は環境によって異なります。利用する形式を決める前に、[カスタム命令のサポート情報][custom-instructions-support]を確認してください。

プロジェクトでは、全体的な説明やコーディング標準を `copilot-instructions.md` に記載し、単体テスト、Astro コンポーネント、データ レイヤーなどの個別ガイダンスを `*.instructions.md` に分けます。自然な言葉で、明確なガイダンスと例を記載してください。

## プロジェクトのカスタム命令を確認する

スターター プロジェクトに含まれる命令ファイルを確認します。

1. Codespace に戻り、`.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明、コーディング ガイダンス、TypeScript 規約、TSDoc コメントに関する **Code standards** などを確認します。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが適用対象の glob を定義していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. 単体テスト作成のガイダンスを確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。他の命令ファイルやプロジェクト内ファイルへのリンクを使うと、大きな命令を再利用可能な単位に分けられます。パスは命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントの要件はありません。次に TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

次に `.github/copilot-instructions.md` に、TypeScript コードのドキュメント標準を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 35 行目付近にある **Code formatting guidance** セクションを探します。
3. そのセクションの最後の箇条書きとして、次の Markdown を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルター機能のコンテキストを保ったまま、新しいガイダンスで会話を再開します。

1. フィルターの会話が開いている場合は `/exit` で終了します。会話は自動的に保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替わっていることを確認し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択肢が表示されたらフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のプロンプトを送信します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- リポジトリの命令とパスごとの命令ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を徹底するために命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルを調整して実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
