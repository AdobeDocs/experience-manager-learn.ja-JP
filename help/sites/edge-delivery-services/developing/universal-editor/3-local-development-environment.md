---
title: ローカル開発環境の設定
description: Edge Delivery Services で配信され、ユニバーサルエディターで編集できるサイトのローカル開発環境を設定します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 66bc4cb6f992c64b1a7e32310ce3e26515f3d380
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 91%

---

# ローカル開発環境を設定

Edge Delivery Services で配信される web サイトを迅速に開発するには、ローカル開発環境が不可欠です。この環境では、ローカルで開発されたコードを使用しながら、Edge Delivery Services からコンテンツを取得するので、開発者はコードの変更を即座に確認できます。このような設定により、高速で反復的な開発とテストがサポートされます。

Edge Delivery Services の web サイトプロジェクトの開発ツールとプロセスは、web 開発者にとって使いやすいように設計されており、高速で効率的な開発エクスペリエンスを実現します。

## 開発トポロジ

このビデオでは、ユニバーサルエディターで編集可能なEdge Delivery Services web サイトプロジェクトの開発トポロジの概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3443978/?learn=on&enablevpops)

+++その他の開発トポロジの詳細を参照してください

- **GitHub リポジトリ**：
   - **目的**：Web サイトのコード（CSS および JavaScript）をホストします。
   - **構造**：**メイン分岐**&#x200B;には実稼動環境対応のコードが含まれ、他の分岐には作業用コードが保持されます。
   - **機能**：任意の分岐のコードは、ライブ web サイトに影響を与えることなく、**実稼動**&#x200B;環境または&#x200B;**プレビュー**&#x200B;環境に対して実行できます。

- **AEM オーサーサービス**：
   - **目的**：Web サイトのコンテンツを編集および管理する正規のコンテンツリポジトリとして機能します。
   - **機能**：**ユニバーサルエディター**&#x200B;でコンテンツの読み取りと書き込みを行います。編集したコンテンツは、**実稼動**&#x200B;環境または&#x200B;**プレビュー**&#x200B;環境で **Edge Delivery Services** に公開されます。

- **ユニバーサルエディター**：
   - **目的**：Web サイトのコンテンツを編集する WYSIWYG インターフェイスを提供します。
   - **機能**：**AEM オーサーサービス**&#x200B;に対して読み取りと書き込みを行います。変更に対するテストおよび検証に、**GitHub リポジトリ**&#x200B;の任意の分岐のコードを使用するように設定できます。

- **Edge Delivery Services**：
   - **実稼動環境**：
      - **目的**：ライブ web サイトのコンテンツとコードをエンドユーザーに配信します。
      - **機能**：**GitHub リポジトリ**&#x200B;の&#x200B;**メイン分岐**&#x200B;のコードを使用して、**AEM オーサーサービス**&#x200B;から公開されたコンテンツを提供します。
   - **プレビュー環境**：
      - **目的**：デプロイメント前にコンテンツとコードをテストおよびプレビューするステージング環境を提供します。
      - **機能**：**GitHub リポジトリ**&#x200B;の任意の分岐のコードを使用して **AEM オーサーサービス**&#x200B;から公開されたコンテンツを提供し、ライブ web サイトに影響を与えることなく徹底的なテストを実現します。

- **ローカル開発環境**：
   - **目的**：開発者がローカルでコード（CSS および JavaScript）を書き込みおよびテストできるようにします。
   - **構造**：
      - 分岐ベースの開発の **GitHub リポジトリ**&#x200B;のローカルクローン。
      - 開発サーバーとして機能する **AEM CLI** は、ホットリロードエクスペリエンスを実現できるように、ローカルコードの変更を&#x200B;**プレビュー環境**&#x200B;に適用します。
   - **ワークフロー**：開発者はローカルでコードを書き込み、作業用分岐に変更をコミットし、その分岐を GitHub にプッシュし、**ユニバーサルエディター**&#x200B;で検証し（指定された分岐を使用）、実稼動環境へのデプロイメントの準備が整ったら&#x200B;**メイン分岐**&#x200B;に結合します。

+++

## 前提条件

開発を開始する前に、次をマシンにインストールします。

1. [Git](https://git-scm.com/)
1. [Node.js と npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/)（または同様のコードエディター）

## GitHub リポジトリのクローンを作成

AEM Edge Delivery Servicesのコードプロジェクトを含む新しいコードプロジェクトの章で作成した [GitHub リポジトリ ](./1-new-code-project.md) を、ローカル開発環境に複製します。

![GitHub リポジトリのクローン](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

プロジェクトのルートとして機能する `Code` ディレクトリに新しい `aem-wknd-eds-ue` フォルダーが作成されます。マシン上の任意の場所にプロジェクトのクローンを作成できますが、このチュートリアルではルートディレクトリとして `~/Code` を使用します。

## プロジェクトの依存関係のインストール

プロジェクトフォルダーに移動し、`npm install` を使用して必要な依存関係をインストールします。Edge Delivery Services プロジェクトでは、Webpack や Vite などの従来の Node.js ビルドシステムは使用されませんが、ローカル開発には引き続きいくつかの依存関係が必要です。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM CLI をインストールします。

AEM CLI は、Edge Delivery Services ベースの AEM web サイトの開発を効率化するように設計された Node.js コマンドラインツールであり、web サイトの迅速な開発とテストのローカル開発サーバーを提供します。

AEM CLI をインストールするには、次を実行します。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI は、`npm install --global @adobe/aem-cli` を使用してグローバルにインストールすることもできます。

## ローカル AEM 開発サーバーの起動

`aem up` コマンドを実行すると、ローカル開発サーバーが起動し、サーバーの URL へのブラウザーウィンドウが自動的に開きます。このサーバーは、Edge Delivery Services 環境へのリバースプロキシとして機能し、開発にローカルコードベースを使用しながら、そこからコンテンツを提供します。

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

AEM CLI により、ブラウザーで web サイト `http://localhost:3000/` が開きます。プロジェクトの変更は web ブラウザーに自動的にホットリロードされますが、コンテンツの変更には[プレビュー環境に公開](./6-author-block.md)し、web ブラウザーを更新する必要があります。

Web サイトが 404 ページで開いた場合は、[ 新規コードプロジェクト ](./1-new-code-project.md) で更新された [fstab.yaml または paths.json](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) が誤って設定されているか、変更が `main` ブランチにコミットされていない可能性があります。

## JSON フラグメントの作成

[AEM ボイラープレート XWalk テンプレート](https://github.com/adobe-rnd/aem-boilerplate-xwalk)を使用して作成された Edge Delivery Services プロジェクトは、ユニバーサルエディターでのブロックオーサリングを有効にする JSON 設定に依存します。

- **JSON フラグメント**：関連するブロックと共に保存され、ブロックモデル、定義およびフィルターを定義します。
   - **モデルフラグメント**：`/blocks/example/_example.json` に保存されます。
   - **定義フラグメント**：`/blocks/example/_example.json` に保存されます。
   - **フィルターフラグメント**：`/blocks/example/_example.json` に保存されます。

NPM スクリプトは、これらの JSON フラグメントをコンパイルし、プロジェクトルートの適切な場所に配置します。JSON ファイルを作成するには、提供されている NPM スクリプトを使用します。例えば、すべてのフラグメントをコンパイルするには、次を実行します。

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM スクリプト | 説明 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | フラグメントからすべての JSON ファイルを作成し、適切な `component-*.json` ファイルに追加します。 |
| `build:json:models` | ブロック JSON フラグメントを作成し、`/component-models.json` にコンパイルします。 |
| `build:json:definitions` | ページ JSON フラグメントを作成し、`/component-definitions.json` にコンパイルします。 |
| `build:json:filters` | ページ JSON フラグメントを作成し、`/component-filters.json` にコンパイルします。 |

>[!TIP]
>
> フラグメントファイルに変更を行った後は、`npm run build:json` を実行して、メイン JSON ファイルを再生成します。

## リンティング

リンティングにより、変更を `main` 分岐に結合する前に Edge Delivery Services プロジェクトに必要なコードの品質と一貫性が確保されます。

NPM スクリプトは、`npm run` 経由で実行できます。次に例を示します。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM スクリプト | 説明 |
|------------------|--------------------------------------------------|
| `lint:js` | JavaScript リンティングチェックを実行します。 |
| `lint:css` | CSS リンティングチェックを実行します。 |
| `lint` | JavaScript と CSS の両方のリンティングチェックを実行します。 |

### リンティングの問題を自動的に修正

次の `scripts` をプロジェクトの `package.json` に追加し、`npm run` 経由で実行して、リンティングの問題を自動的に解決できます。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

これらのスクリプトは、AEM ボイラープレート XWalk テンプレートに事前設定されていませんが、`package.json` ファイルに追加できます。

>[!BEGINTABS]

>[!TAB 追加スクリプト]

| NPM スクリプト | コマンド | 説明 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | JavaScript リンティングの問題を自動的に修正します。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | CSS リンティングの問題を自動的に修正します。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 迅速なクリーンアップに、JS と CSS の両方の修正スクリプトを実行します。 |

>[!TAB package.json の例]

次のスクリプトエントリを `package.json` `scripts` 配列に追加できます。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
