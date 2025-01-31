---
title: ローカル開発環境を設定
description: Edge Delivery Servicesが配信され、ユニバーサルエディターで編集可能なサイトのローカル開発環境を設定します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 6f0cbdd638ed909b5897521557b65dcf74ac1012
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 2%

---

# ローカル開発環境を設定

Edge Delivery Servicesが提供する web サイトを迅速に開発するには、ローカル開発環境が不可欠です。 この環境では、Edge Delivery Servicesからコンテンツを取得しながら、ローカルで開発されたコードを使用するため、デベロッパーはコードの変更内容を即座に確認できます。 このような設定により、迅速で反復的な開発とテストがサポートされます。

Edge Delivery Servicesweb サイトプロジェクトの開発ツールとプロセスは、web 開発者に慣れ親しみ、迅速かつ効率的な開発経験を提供するように設計されています。

## 開発トポロジ

ユニバーサルエディターで編集可能なEdge Delivery Services Web サイトプロジェクトの開発トポロジは、次の側面で構成されます。

- **GitHub リポジトリ**:
   - **目的**:Web サイトのコード（CSS およびJavaScript）をホストします。
   - **構造**: **main ブランチ** には実稼動用の準備完了コードが含まれ、その他のブランチには作業用コードが保持されます。
   - **機能**：任意のブランチのコードを、ライブ Web サイトに影響を与えることなく、**実稼動** または **プレビュー** 環境に対して実行できます。

- **AEM オーサーサービス**:
   - **目的**:web サイトのコンテンツを編集および管理する正規のコンテンツリポジトリとして機能します。
   - **機能**：コンテンツの読み取りと書き込みは **ユニバーサルエディター** で行います。 編集されたコンテンツは、{production **または** preview **Edge Delivery Servicesの** 0 **環境に公開されます。**

- **ユニバーサルエディター**:
   - **目的**:web サイトのコンテンツを編集するためのWYSIWYG インターフェイスを提供します。
   - **機能**:**AEM オーサーサービスの読み取りと書き込み**。 **GitHub リポジトリ** の任意のブランチのコードを使用するように設定し、変更をテストおよび検証できます。

- **Edge Delivery Services**:
   - **実稼動環境**:
      - **目的**：ライブ web サイトのコンテンツとコードをエンドユーザーに配信します。
      - **機能**:{GitHub リポジトリ **の** main ブランチ **のコードを使用して****AEM オーサーサービスから公開されたコンテンツを提供します**
   - **プレビュー環境**:
      - **目的**：デプロイメント前にコンテンツとコードをテストおよびプレビューするステージング環境を提供します。
      - **機能**:{GitHub リポジトリ **のいずれかのブランチのコードを使用して** **** AEM オーサーサービスから公開されたコンテンツを提供し、ライブ Web サイトに影響を与えずに徹底したテストを可能にします。

- **ローカル開発環境**:
   - **目的**：開発者がコード（CSS およびJavaScript）をローカルに記述およびテストできるようにします。
   - **構造**:
      - ブランチベースの開発用の **GitHub リポジトリ** のローカルクローン。
      - 開発サーバーとして機能する **AEM CLI** は、ホットリロードエクスペリエンス用にローカルコードの変更を **プレビュー環境** に適用します。
   - **ワークフロー**：開発者は、実稼動デプロイメントの準備が整ったら、コードをローカルで記述し、変更を作業ブランチにコミットして、ブランチを GitHub にプッシュし、**ユニバーサルエディター** で（指定されたブランチを使用して）検証して、**メインブランチ** に結合します。

## 前提条件

開発を開始する前に、以下をコンピューターにインストールします。

1. [Git](https://git-scm.com/)
1. [Node.js および npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) （または同様のコードエディター）

## GitHub リポジトリのクローン

AEM Edge Delivery Servicesのコードプロジェクトを含む新しいコードプロジェクトの章で作成した [GitHub リポジトリ ](./1-new-code-project.md) を、ローカル開発環境に複製します。

![GitHub リポジトリのクローン ](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

`Code` ディレクトリに、プロジェクトのルートとして新しい `aem-wknd-eds-ue` フォルダーが作成されます。 プロジェクトのクローンをマシン上の任意の場所に作成できますが、このチュートリアルでは `~/Code` をルートディレクトリとして使用します。

## プロジェクトの依存関係のインストール

プロジェクトフォルダーに移動し、`npm install` で必要な依存関係をインストールします。 Edge Delivery Servicesプロジェクトでは、Webpack や Vite などの従来の Node.js ビルドシステムは使用されませんが、ローカル開発には引き続き複数の依存関係が必要です。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM CLI をインストールします。

AEM CLI は Node.js のコマンドラインツールであり、Edge Delivery ServicesベースのAEM web サイトの開発を合理化して、web サイトの迅速な開発とテストのためのローカル開発サーバーを提供するように設計されています。

AEM CLI をインストールするには、次のコマンドを実行します。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI は、`npm install --global @adobe/aem-cli` を使用してグローバルにインストールすることもできます。

## ローカル AEM開発サーバーを起動します。

`aem up` コマンドを実行すると、ローカル開発サーバーが起動し、そのサーバーの URL を示すブラウザーウィンドウが自動的に開きます。 このサーバーは、Edge Delivery Services環境のリバースプロキシとして機能し、開発用のローカルコードベースを使用しながら、そこからコンテンツを提供します。

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

AEM CLI を使用すると、ブラウザーの `http://localhost:3000/` に web サイトが開きます。 プロジェクト内の変更は、コンテンツの変更 [ プレビュー環境への公開が必要）と web ブラウザーの更新 ](./6-author-block.md) に応じて、web ブラウザーで自動的にホットリロードされます。

Web サイトが 404 ページで開いた場合は、[ 新規コードプロジェクト ](./1-new-code-project.md) で更新された [fstab.yaml または paths.json](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) が誤って設定されているか、変更が `main` ブランチにコミットされていない可能性があります。

## JSON フラグメントの作成

[AEM Boilerplate XWalk テンプレート ](https://github.com/adobe-rnd/aem-boilerplate-xwalk) を使用して作成されたEdge Delivery Servicesプロジェクトは、ユニバーサルエディターでブロックオーサリングを有効にする JSON 設定に依存しています。

- **JSON フラグメント**：関連するブロックと共に保存され、ブロックモデル、定義およびフィルターを定義します。
   - **モデルフラグメント**:`/blocks/example/_example.json` に保存されます。
   - **定義フラグメント**:`/blocks/example/_example.json` に保存されます。
   - **フィルターフラグメント**:`/blocks/example/_example.json` に保存されます。

NPM スクリプトは、これらの JSON フラグメントをコンパイルして、プロジェクトルートの適切な場所に配置します。 JSON ファイルを作成するには、提供された NPM スクリプトを使用します。 例えば、すべてのフラグメントをコンパイルするには、次を実行します。

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM スクリプト | 説明 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | フラグメントからすべての JSON ファイルをビルドし、適切な `component-*.json` ファイルに追加します。 |
| `build:json:models` | ブロック JSON フラグメントをビルドし、`/component-models.json` にコンパイルします。 |
| `build:json:definitions` | ページの JSON フラグメントをビルドし、`/component-definitions.json` にコンパイルします。 |
| `build:json:filters` | ページの JSON フラグメントをビルドし、`/component-filters.json` にコンパイルします。 |

>[!TIP]
>
> フラグメントファイルに変更が加えられた場合は、`npm run build:json` を実行してメインの JSON ファイルを再生成します。

## リンティング

リンティングは、変更を `main` ブランチにマージする前にEdge Delivery Servicesプロジェクトで必要になる、コードの品質と一貫性を確保します。

NPM スクリプトは、`npm run` を使用して実行できます。次に例を示します。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM スクリプト | 説明 |
|------------------|--------------------------------------------------|
| `lint:js` | JavaScript リンティングチェックを実行します。 |
| `lint:css` | CSS リンティングチェックを実行します。 |
| `lint` | JavaScriptと CSS の両方のリンティングチェックを実行します。 |

### リンティングの問題を自動的に修正

次の `scripts` をプロジェクトの `package.json` に追加すると、リンティングの問題を自動的に解決でき、`npm run` を使用して実行できます。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

これらのスクリプトは、AEM Boilerplate XWalk テンプレートで事前設定されていませんが、`package.json` ファイルに追加できます。

>[!BEGINTABS]

>[!TAB  追加スクリプト ]

| NPM スクリプト | コマンド | 説明 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js --fix` | JavaScriptのリンクの問題を自動的に修正します。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css --fix` | CSS リンティングの問題を自動的に修正します。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | クイッククリーンアップ用に JS と CSS の両方の fix スクリプトを実行します。 |

>[!TAB package.json] 例

次のスクリプトエントリを `package.json` `scripts` 配列に追加できます。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js --fix",
    "lint:css:fix": "npm run lint:css --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
