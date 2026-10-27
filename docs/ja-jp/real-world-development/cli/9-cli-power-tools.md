---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト、モデル、セッション共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には多くの slash command があり、高度な機能や設定を利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、さらに便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキストを確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を確認します。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、slash command で確認や圧縮を行うこともできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを選ぶ場合は `/compact` を使います。無関係なタスクに移る場合は `/clear` または `/new` で新しい会話を始めます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI で利用できるモデルを表示し、選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` の選択肢を確認します。
2. 共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った出力先を選びます。
3. エクスポートした内容を共有または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業の拡張や委任

コア ワークショップは完了です。大きなタスクには、次の Copilot CLI コマンドも利用できます。

- `/fleet` は独立したサブタスクを複数のサブエージェントに分割し、並列実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プル リクエストを作成する場合があります。

これらのコマンドは worktree やリモート作業を追加する可能性があるため任意です。試す場合は、範囲の明確な新しいタスクから始め、通常の手順で結果をレビューしてください。非同期エージェントの作業について詳しく知るには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキストを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を確認しました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

次は[ここまでの内容を振り返り][next-lesson]、学習を続けるためのリソースを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
