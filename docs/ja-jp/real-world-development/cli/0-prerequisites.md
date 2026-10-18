---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用に GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、codespace の統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ用リポジトリを設定する

自分用の Tailspin Toys プロジェクトを使います。[テンプレート リポジトリ][tailspin-template]から今すぐ作成してください。新しいリポジトリにはラボに必要なすべてのファイルが含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template**、**Create a new repository** の順に選択して、自分用のリポジトリを作成します。
3. GitHub または Microsoft が主催するイベントで受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 後の演習で参照するため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこれらの Issue を使うため、自分で起票する必要はありません。

新しいワークショップ テンプレートのコピーを使ってください。テンプレートには、リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、演習で使うバックログが含まれています。古いコピーを使う場合は、必要なファイルがそろっているかファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートするフル機能のエディターが用意されています。

1. 新しく作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカル データベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択し、Tailspin Toys のサイトが表示されることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
