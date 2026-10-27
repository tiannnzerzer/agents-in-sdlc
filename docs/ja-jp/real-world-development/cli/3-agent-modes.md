---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue に基づくフィルター機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、さらに体系的なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って利用できます。このレッスンから、issue を起点に機能を作成し、コードの妥当性と動作を確認し、プロジェクトにマージするまでの、エージェントを使った開発プロセスを進めます。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチやプル リクエスト (PR) を分けますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この後のレッスンでは、リポジトリの命令を更新し、既存の `quality-checks` スキルを調整し、Model Context Protocol (MCP) による検証を追加し、品質保証 (QA) エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリとパブリッシャーでゲームを絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要です。Plan モードで判断を明確にしてから、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、ソフトウェア開発の基本は変わりません。一般的な流れは次のとおりです。

1. 作業内容を記載した issue から始めます。
2. 実装計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合はここに挙げた手順を基本とします。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更を提案し、次の操作の前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画をレビューし、その後 Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトに `#7` のような番号が追加されます。`#` を使うと、GitHub 上の issue やプル リクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を任せる前に機能の方針を決められます。複雑な作業では、計画に時間をかけることをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、実装方針を検討します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画をレビューしたら、Autopilot で実装します。Autopilot は完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** または、インストール済みバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係がない、またはポートが競合していると報告された場合は、チェック完了とみなす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を issue と計画時の判断に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

Codespaces の転送ポートからサイトを開き、機能が動作することを確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. ポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、パブリッシャー、それらの組み合わせで絞り込みます。
4. リセットと該当結果がない場合の動作が issue と計画に合うことを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム命令を使ってコード生成をガイドし][next-lesson]、文書化された規約に沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
