---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作り、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を設定して、特定の作業を効率よく進められます。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントはプロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept] は、どちらも Markdown ファイルを使って AI に作業方法を伝えるため、用途が重なる部分があります。簡単に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整して作業できます。この演習のアクセシビリティエージェントは、アクセシビリティガイドラインに照らしてサイトをレビュー、更新します。その際に、PR ワークフローやテストの実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作る方法に唯一の正解はありません。AI の他の利用方法と同様に、試して改善し、環境やシナリオに合う方法を見つけてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングのサイトを利用できるようにしたいと考えています。現在のダークテーマは、テキストと背景のコントラストが不十分で読みにくいというフィードバックが寄せられています。デザインチームは、ユーザーが切り替えられるハイコントラストモードを追加したいと考えています。

アクセシビリティは重要なため、カスタムエージェントを使ってできるだけ早く機能を作成します。

この演習では、次のことを行います。

- アクセシビリティ用のカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認し、Copilot をどう導くか理解します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルター機能の PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 後続のセクションを確認します。アクセシブルな Web サイトのコードを生成する際の責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある間違いなどが記載されています。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` にあるカスタムエージェントが Copilot Chat ビュー下部のエージェント一覧に表示されます。エージェントを選択すると、その指示とツールを使うよう会話を設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選んで新しい会話を始めます。モードとモデルの選択は演習によって異なるため、各手順で指定します。

1. エージェント一覧で **Agent** が選択されていることを確認します。選択されていない場合は選択します。
2. チャットビュー下部にあるエージェント一覧を開きます。既定では **default** が選択されています。
3. 一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーが設定できる切り替え機能を用意し、ブラウザーの local storage を使ってページの再読み込み後も設定を保持してください。
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトのファイルを編集します。編集ごとにチャット内でファイルパスと差分を確認できます。
6. エージェントが変更したファイルが増えると、**Files changed** の表示も更新されます。

> [!NOTE]
> 処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどを実際に変更しています。

次の演習では、この作業の進行状況を確認し、追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を使い、特定のタスクや分野に合わせたエージェントを確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめられます。

- カスタムエージェントの定義方法を学びました。
- Copilot Chat のエージェントモードでカスタムエージェントを使いました。

次は[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

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
