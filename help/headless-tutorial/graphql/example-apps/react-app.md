---
title: React アプリ - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この React アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。
version: Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
duration: 256
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 100%

---

# React アプリ{#react-app}

サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この React アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。 JavaScript 用 AEM ヘッドレスクライアントは、アプリを強化する GraphQL の永続クエリを実行するために使用されます。

![AEM ヘッドレスを使用した React アプリ](./assets/react-app/react-app.png)

[GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)を表示

この React アプリがどのようにビルドされたかを説明する[詳細な手順のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja)を使用できます。

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

+ [Node.js v18](https://nodejs.org/ja/)
+ [Git](https://git-scm.com/)

## AEM の要件

React アプリケーションは、次の AEM デプロイメントオプションと連携します。 すべてのデプロイメントに [WKND Site v3.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)をインストールする必要があります。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) を使用したローカル設定
   + [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/jp/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) が必要

React アプリケーションは __AEM パブリッシュ__&#x200B;環境で使用できますが、React アプリケーションの設定で認証が提供されている場合は、AEM オーサーからコンテンツをソース化できます。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `aem-guides-wknd-graphql/react-app/.env.development` ファイルを編集して、`REACT_APP_HOST_URI` がターゲットの AEM を指定するようにします。

   オーサーインスタンスに接続する場合は、認証方法を更新します。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. ターミナルを開き、次のコマンドを実行します。

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウで [http://localhost:3000](http://localhost:3000) が読み込まれます。
1. WKND 参照サイトのアドベンチャーのリストがアプリケーションに表示されます。

## コード

次に、React アプリケーションがビルドされる方法、GraphQL での永続化クエリを使用してコンテンツを取得するために AEM ヘッドレスに接続する方法、およびそのデータが表示される方法の概要を示します。 完全なコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) にあります。


### 永続クエリ

AEM ヘッドレスのベストプラクティスに従い、React アプリケーションは AEM GraphQLで永続クエリを使用してアドベンチャーデータをクエリします。 アプリケーションでは、次の 2 つの永続クエリを使用します。

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

+ `wknd/adventure-by-slug` 永続クエリ。`slug`（アドベンチャーを一意に識別するカスタムプロパティ）によって、完全なプロパティセットを含む単一のアドベンチャーを返します。この永続クエリで、アドベンチャーの詳細ビューが強化されます。

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL 永続クエリの実行

AEM の永続クエリは HTTP GET を介して実行され、[JavaScript 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js)を使用して、AEM に対して[永続 GraphQL クエリを実行](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)し、アドベンチャーコンテンツをアプリに読み込みます。

各永続クエリには、対応する React [useEffect](https://reactjs.org/docs/hooks-effect.html) フックが `src/api/usePersistedQueries.js` にあり、AEM HTTP GET永続クエリエンドポイントを非同期的に呼び出して、アドベンチャーデータを返します。

各関数は順番に `aemHeadlessClient.runPersistedQuery(...)` に呼び出し、GraphQL 永続クエリを実行します。

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### ビュー

React アプリケーションは、2 つのビューを使用して、web エクスペリエンスにアドベンチャーデータを表示します。

+ `src/components/Adventures.js`

  `src/api/usePersistedQueries.js` から `getAdventuresByActivity(..)` を呼び出して、返されたアドベンチャーをリストに表示します。

+ `src/components/AdventureDetail.js`

  `Adventures` コンポーネントでアドベンチャー選択経由で渡される `slug` パラメーターを使用して `getAdventureBySlug(..)` を呼び出し、単一のアドベンチャーの詳細を表示します。

### 環境変数

複数の[環境変数](https://create-react-app.dev/docs/adding-custom-environment-variables)を使用して AEM 環境に接続します。デフォルトでは `http://localhost:4503` で実行されている AEM パブリッシュに接続します。`.env.development` ファイルを更新し、AEM 接続を変更します。

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`：AEM ターゲットホストに設定します
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`：GraphQL エンドポイントのパスを設定します。このアプリは永続クエリのみを使用するので、この React アプリでは使用されません。
+ `REACT_APP_AUTH_METHOD=`：推奨される認証方法。オプションであるため、デフォルトでは認証は使用されません。
   + `service-token`：サービス資格情報を使用して AEM as a Cloud Service でアクセストークンを取得します
   + `dev-token`：AEM as a Cloud Service でローカル開発に開発トークンを使用します
   + `basic`：ローカル AEM オーサーとのローカル開発にユーザー／パスを使用します
   + 認証なしで AEM に接続する場合は空白のままにします
+ `REACT_APP_AUTHORIZATION=admin:admin`：AEM オーサー環境に接続する場合に、使用する基本認証の資格情報を設定します（開発用のみ）。 パブリッシュ環境に接続する場合、この設定は不要です。
+ `REACT_APP_DEV_TOKEN`：開発トークン文字列。リモートインスタンスに接続するには、基本認証（ユーザー：パス）の他に、クラウドコンソールから開発トークンで Bearer 認証を使用できます
+ `REACT_APP_SERVICE_TOKEN`：サービス資格情報ファイルへのパス。リモートインスタンスに接続する場合は、サービストークン（Developer Console からファイルをダウンロード）を使用しても認証を行えます。

### プロキシ AEM リクエスト

WebPack 開発サーバーを使用する場合（`npm start`）、プロジェクトは `http-proxy-middleware` を使用した[プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)に依存します。ファイルは [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) で設定され、`.env` および `.env.development` で設定された複数のカスタム環境変数に依存しています。

AEM オーサー環境に接続する場合、[認証方法を設定する必要があります](#environment-variables)。

### クロスオリジンリソース共有（CORS）

この React アプリケーションは、ターゲット AEM 環境で動作する AEM ベースの CORS 設定に依存し、React アプリが `http://localhost:3000` で開発モードで動作することを前提としています。CORS をセットアップして設定する方法について詳しくは、[AEM ヘッドレスデプロイメントのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html?lang=ja)を参照してください。
