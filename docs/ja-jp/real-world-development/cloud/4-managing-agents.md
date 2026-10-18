---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー体験の改善や機能追加を目的とした 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の [agents ページ][agents-page]などから管理できます。この mission control では、オープンなプルリクエスト (PR) があるエージェントのタスクを確認できます。実行内容を確認し、実行中のセッションに指示を追加することもできます。

この演習では、次のことを行います。

- agents ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに指示を送り、機能追加を依頼します。

## シナリオ

チームはハイコントラストモードに加え、ライトモードも追加することにしました。サイトのスタイルと切り替え機能を変更している最中なので、エージェントに作業を追加で依頼し、両方のモードを実装してもらいます。

## Copilot cloud agent のタスクを確認する

cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の一覧を確認します。割り当てたタスクとして、次のようなものが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更する API を作成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。たとえば、次のような項目があります。
   - リポジトリをチェックアウトする。
   - 作業環境を作成する。
   - MCP サーバーを設定する。
   - タスクを完了するための手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した方法によって異なります。

4. 右側に表示される PR パネルも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したところで、エージェントにライトモードも追加するよう依頼します。

1. ハイコントラストモードのタスクを示すセッションを選択します。表示されるタイトルは Copilot の命名や作業状況に応じて異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、実際の作業が始まるまで数分待ちます。次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードに加えてライトモードも追加してください。ユーザーが希望する表示モードを選べるように、ライトモード用の切り替え機能も用意してください。
    ```

    ![agents ページの cloud agent タスクでセッションの方向付けダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に反映することを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信したときに作業中だった手順が完了すると、新しい依頼を作業に取り込みます。

今回も完了まで数分かかります。その間に、ここまでに学んだ内容を振り返ってください。

## まとめと次のステップ

この演習では、Copilot agents ページを使って cloud agent のタスクを監視し、作業中のセッションに機能追加を依頼しました。

次の内容を学びました。

- mission control と agents ページで cloud agent のタスクを監視する方法。
- 実行中のセッションに追加の指示を送る方法。

アクセシビリティ機能の作業が完了したら、次は [Copilot が作成したプルリクエストをレビューして改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
