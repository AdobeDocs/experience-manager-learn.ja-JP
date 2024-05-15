---
title: サーバー間 Node.js アプリ - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 このサーバー側 Node.js アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツを照会する方法を示しています。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 135
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 100%

---

# サーバー間 Node.js アプリ

サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 このサーバー間アプリケーションでは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行し、そのクエリをターミナルに印刷する方法を示します。

![AEM ヘッドレスを備えたサーバー間 Node.js アプリ](./assets/server-to-server-app/server-to-server-app.png)

[GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)を表示

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

+ [Node.js v18](https://nodejs.org/ja)
+ [Git](https://git-scm.com/)

## AEM の要件

Node.js アプリケーションは、次の AEM デプロイメントオプションと連携します。すべてのデプロイメントに [WKND Site v3.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)をインストールする必要があります。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ オプションで、[サービス資格情報](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=ja)リクエストを承認することもできます（例えば、AEM オーサーサービスへの接続）。

この Node.js アプリケーションは、コマンドラインパラメーターに基づいて、AEM オーサーまたは AEM パブリッシュに接続できます。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. ターミナルを開き、次のコマンドを実行します。

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. デスクトップアプリケーションは、次のコマンドを使用して実行できます。

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例えば、認証なしで AEM パブリッシュに対してアプリを実行するには、次のようにします。

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   認証を持つ AEM オーサーに対してアプリを実行します。

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. WKND リファレンスサイトから、アドベンチャーの JSON リストを端末に出力する必要があります。

## コード

サーバー間 Node.js アプリケーションの構築方法、GraphQL で保持されたクエリを使用してコンテンツを取得する AEM ヘッドレスへの接続方法およびそのデータの表示方法の概要を次に示します。完全なコードは [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server) にあります。

サーバー間 AEM ヘッドレスアプリの一般的なユースケースは、AEM のコンテンツフラグメントデータを他のシステムに同期することですが、このアプリケーションは意図的に単純で、永続クエリから JSON 結果を出力します。

### 永続クエリ

AEM ヘッドレスのベストプラクティスに従い、アプリケーションは AEM GraphQL で永続クエリを使用してアドベンチャーのデータを照会します。アプリケーションでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 永続クエリ。AEM のすべてのアドベンチャーを要約されたプロパティセットとともに返します。この永続クエリは、初期ビューのアドベンチャーリストを制御します。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### AEM ヘッドレスクライアントを作成

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### GraphQL 永続クエリの実行

AEM の永続クエリは HTTP GET で実行されるので、[AEM Node.js 用ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-nodejs)を使用して AEM に対して[永続 GraphQL クエリを実行](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait)してアドベンチャーコンテンツを取得します。

`aemHeadlessClient.runPersistedQuery(...)` を呼び出し、永続 GraphQL クエリ名を渡すことで、永続クエリを呼び出すことができます。GraphQL がデータを返したら、それを簡略化した `doSomethingWithDataFromAEM(..)` 関数に渡します。この関数は結果を表示しますが、通常はデータを別のシステムに送信したり、取得したデータに基づいて何らかの出力を生成したりします。

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
