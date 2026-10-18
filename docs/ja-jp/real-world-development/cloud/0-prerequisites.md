---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。[テンプレート][template-repository]から Tailspin Toys リポジトリを作成し、指示ファイルの編集や cloud agent が作成した成果のレビューに使う [codespace][codespaces] を起動します。

## 演習用リポジトリをセットアップする

コード用のリポジトリを作るには、[テンプレート][template-repository]から新しいリポジトリを作成します。このリポジトリには演習に必要なファイルが含まれます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選び、続けて **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後の演習で使用します。

## codespace を作成する

次に codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを開き、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりずっと早く済みます。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> この codespace は後の演習でも使います。今はブラウザーのタブを閉じずに残してください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらにも演習に必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリを作成し、演習で使う codespace の作成を開始しました。

## 次のステップ

次は cloud agent が従うカスタム命令を追加します。[演習 1 - カスタム命令][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
