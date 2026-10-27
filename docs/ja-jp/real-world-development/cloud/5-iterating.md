---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善して機能を追加する複数のタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザイン チームが改善を検討する関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、直接の監視なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスはコード作業用のブランチに限られます。
- cloud agent は GitHub Actions 内に、独立した一時的な作業環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- Model Context Protocol (MCP) サーバーを含む外部リソースへの[アクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために Copilot cloud agent が作成した最初のプル リクエスト (PR) を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。これは想定どおりです。MCP サーバーへのアクセスを含め、Copilot の外部リソースへのアクセスは既定で制限されています。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は Copilot の作業が完了するまで待ちます。数分かかる場合があるため、その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントなど、更新されたコードを確認します。実際の変更内容は状況によって異なります。レビューでは、演習 2 の最初に確認した命令ファイルが反映されているか、TSDoc コメント、TypeScript 規約、ファイル ヘッダーを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーに依頼する場合と同様に、PR のコメントやコード行へのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず使うプロンプトは指定できません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにメンションし、生成されたドキュメントの変更をコメントで依頼します。上記の例を使うか、コードベースに追加したいドキュメントを依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プル リクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を依頼した issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターがあることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の作業と同様に、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc コメントを追加したりできます。この issue はカスタム命令の更新前に割り当てたため、後者の依頼が必要な場合があります。**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が終わるまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持するための local storage の使用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進める方法を練習します。リポジトリに issue を追加して Copilot に割り当ててください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- リポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力にどう影響するかを調べました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して作業中に方向を調整しました。
- cloud agent の PR をレビューして反復し、**@copilot** に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスは完了です。さらに学ぶ場合は、次のハーネスで今回の内容を別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使って、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のことも試せます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てます。
- 別の分野向けのカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
