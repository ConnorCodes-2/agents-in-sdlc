---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` に Markdown ファイルを置き、作業に必要な指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、テストパターンを統一するテストエージェントを作成できます。

カスタムエージェントはプロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept] は、どちらも主に Markdown ファイルで定義し、AI に作業の実行方法を伝えるため、用途が重なる部分があります。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを使って作業を進められます。この演習ではアクセシビリティエージェントがサイトを確認、更新します。その作業の一環として、プルリクエストのワークフローやテストの実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同じように、試して改善し、環境やシナリオに合うものを見つけてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次のことを行います。

- カスタムエージェントの定義方法を確認します。
- タスクをカスタムエージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングのプラットフォームを利用できるようにしたいと考えています。現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックが寄せられています。そこでデザインチームは、ユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、この機能をできるだけ早く実装したいと考えています。カスタムエージェントを使って機能を作成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認し、Copilot をどのように導くかを理解します。

codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する場所です。cloud agent にタスクを割り当て、監視し、実行中のセッションに追加の指示を送れます。まず Copilot にハイコントラストモードの作成を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードが必要です。ユーザーが設定できる切り替え機能を追加し、ブラウザーの local storage に設定を保存してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとアクセシビリティが選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

この演習では、特定のタスクや分野に特化した GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次の内容を学びました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードを実装している間に、次は [mission control からエージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
