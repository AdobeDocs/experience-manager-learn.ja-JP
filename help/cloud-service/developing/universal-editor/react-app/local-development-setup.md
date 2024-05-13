---
title: ローカル開発設定
description: ユニバーサルエディターのローカル開発環境を設定して、サンプル React アプリのコンテンツを編集できるようにする方法を説明します。
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 3%

---


# ローカル開発設定

AEM ユニバーサルエディターを使用して React アプリのコンテンツをローカル開発する編集環境を設定する方法について説明します。

## 前提条件

このチュートリアルに従うには、以下が必要です。

- HTMLと JavaScript の基本的なスキル。
- 以下のツールをローカルにインストールする必要があります。
   - [Node.js](https://nodejs.org/ja/download/)
   - [Git](https://git-scm.com/downloads)
   - IDE またはコードエディター（例：） [Visual Studio Code](https://code.visualstudio.com/)
- 次をダウンロードしてインストールします。
   - [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk)：開発目的でAEM オーサーとパブリッシュをローカルに実行するために使用されるクイックスタート JAR が含まれています。
   - [ユニバーサルエディターサービス](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home)：ユニバーサルエディターサービスのローカルコピーで、機能のサブセットを備えており、ソフトウェア配布ポータルからダウンロードできます。
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy):ローカル開発用の自己署名証明書を使用したシンプルなローカル SSL HTTP プロキシ。 AEM ユニバーサルエディターで React アプリをエディターに読み込むには、そのアプリの HTTPS URL が必要です。

## ローカル設定

ローカル開発環境をセットアップするには、次の手順に従います。

### AEM SDK

WKND Teams React アプリのコンテンツを提供するには、ローカルのAEM SDK に次のパッケージをインストールします。

- [WKND Teams - コンテンツパッケージ](./assets/basic-tutorial-solution.content.zip)：コンテンツフラグメントモデル、コンテンツフラグメントおよび永続GraphQL クエリが含まれます。
- [WKND Teams – 設定パッケージ](./assets/basic-tutorial-solution.ui.config.zip)：クロスオリジンリソース共有（CORS）およびトークン認証ハンドラー設定が含まれます。 CORS は、AEM以外の web プロパティがAEM GraphQL API に対してブラウザーベースのクライアントサイド呼び出しを行うのを容易にし、トークン認証ハンドラーを使用してAEMに対して各リクエストを認証します。

  ![WKND Teams - パッケージ](./assets/wknd-teams-packages.png)

### React アプリ

WKND Teams React アプリを設定するには、次の手順に従います。

1. のクローン [WKND Teams React アプリ](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) から `basic-tutorial` ソリューションブランチ。

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. に移動します。 `basic-tutorial` ディレクトリを選択し、コードエディターで開きます。

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. 依存関係をインストールし、React アプリを起動します。

   ```bash
   $ npm install
   $ npm start
   ```

1. お使いのブラウザーで WKND チームの React アプリ（）を開きます。 [http://localhost:3000](http://localhost:3000). チームメンバーとその詳細のリストが表示されます。 AEM React アプリのコンテンツは、GraphQL API （`/graphql/execute.json/my-project/all-teams`）、ブラウザーの「ネットワーク」タブを使用して確認できます。

   ![WKND Teams - React アプリ](./assets/wknd-teams-react-app.png)

### ユニバーサルエディターサービス

を設定するには **ローカル** ユニバーサルエディターサービスは、次の手順に従います。

1. から最新バージョンのユニバーサルエディターサービスをダウンロードします [ソフトウェア配布ポータル](https://experience.adobe.com/jp/downloads).

   ![ソフトウェア配布 – ユニバーサルエディターサービス](./assets/universal-editor-service.png)

1. ダウンロードした zip ファイルを解凍し、 `universal-editor-service.cjs` という名前の新しいディレクトリにファイルを送信します。 `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. 作成 `.env` 内のファイル `universal-editor-service` ディレクトリに移動し、次の環境変数を追加します。

   ```bash
   # The port on which the Universal Editor service runs
   EXPRESS_PORT=8000
   # Disable SSL verification
   NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

1. ローカルのユニバーサルエディターサービスを開始します。

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

上記のコマンドは、ポートでユニバーサルエディターサービスを開始します `8000` 次の出力が表示されます。

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### ローカル SSL HTTP プロキシ

AEM ユニバーサルエディターでは、React アプリを HTTPS で提供する必要があります。 ローカル開発に自己署名証明書を使用するローカル SSL HTTP プロキシを設定しましょう。

次の手順に従って、ローカル SSL HTTP プロキシを設定し、HTTPS でAEM SDK とユニバーサルエディターサービスを提供します。

1. のインストール `local-ssl-proxy` グローバルにパッケージ化します。

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. 次のサービス用のローカル SSL HTTP プロキシの 2 つのインスタンスを開始します。

   - ポート上のAEM SDK ローカル SSL HTTP プロキシ `8443`.
   - ポート上のユニバーサルエディターサービスのローカル SSL HTTP プロキシ `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### HTTPS を使用するように React アプリを更新する

WKND Teams React アプリに対して HTTPS を有効にするには、次の手順に従います。

1. 次を押して、React を停止します `Ctrl + C` ターミナルの中です。
1. を更新 `package.json` を含めるファイル `HTTPS=true` の環境変数 `start` スクリプト。

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. を更新 `REACT_APP_HOST_URI` が含まれる `.env.development` AEM SDK の HTTPS プロトコルとローカル SSL HTTP プロキシポートを使用するファイル。

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. を更新 `../src/proxy/setupProxy.auth.basic.js` を使用したリラックスした SSL 設定を使用するファイル `secure: false` オプション。

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. React アプリを起動します。

   ```bash
   $ npm start
   ```

## 設定の確認

上記の手順でローカル開発環境を設定した後、設定を確かめてみましょう。

### ローカル検証

次のサービスが HTTPS 経由でローカルに実行されていることを確認します。自己署名証明書については、ブラウザーでセキュリティ警告を受け入れる必要がある場合があります。

1. の WKND チーム React アプリ [https://localhost:3000](https://localhost:3000)
1. AEM SDK オン [https://localhost:8443](https://localhost:8443)
1. のユニバーサルエディターサービス [https://localhost:8001](https://localhost:8001)

### ユニバーサルエディターでの WKND Teams React アプリの読み込み

設定を確認するために、ユニバーサルエディターで WKND チームの React アプリを読み込みましょう。

1. ブラウザーでユニバーサルエディターhttps://experience.adobe.com/#/aem/editorを開きます。 プロンプトが表示されたら、Adobe IDを使用してログインします。

1. ユニバーサルエディターのサイト URL 入力フィールドに WKND Teams React アプリ URL を入力し、 `Open`.

   ![ユニバーサルエディター – サイト URL](./assets/universal-editor-site-url.png)

1. WKND チーム React アプリがユニバーサルエディターに読み込まれる **ただし、まだコンテンツを編集できません**. ユニバーサルエディターを使用してコンテンツ編集を有効にするには、React アプリを実装する必要があります。

   ![ユニバーサルエディター – WKND Teams React アプリ](./assets/universal-editor-wknd-teams.png)


## 次の手順

方法を学ぶ [react アプリを実装してコンテンツを編集します](./instrument-to-edit-content.md).
