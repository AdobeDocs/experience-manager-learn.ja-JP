---
title: React アプリ — AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この React アプリケーションは、永続化されたクエリを使用してAEM GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 3ef802d4e87b7dc8132dae25c9dbb801dfdce0fe
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 6%

---

# React アプリ

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この React アプリケーションは、永続化されたクエリを使用してAEM GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。 JavaScript 版AEMヘッドレスクライアントは、アプリを強化する GraphQL の永続クエリを実行するために使用されます。

![AEMヘッドレスを使用した React アプリ](./assets/react-app/react-app.png)

次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [ステップごとの完全なチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja) この React アプリのビルドを使用する方法を説明します。

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10 以降](https://nodejs.org/ja/)
+ [npm 6 以降](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要件

React アプリケーションは、次のAEMデプロイメントオプションと連携します。 すべてのデプロイメントには、 [WKND サイト v2.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest) をインストールします。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances)

React アプリケーションは、 __AEM パブリッシュ__ 環境で使用できますが、React アプリケーションの設定で認証が提供されている場合は、AEM オーサーからコンテンツをソース化できます。

## 使用方法

1. のクローン `adobbe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. を編集します。 `aem-guides-wknd-graphql/react-app/.env.development` ファイルとセット `REACT_APP_HOST_URI` をクリックして、ターゲットAEMを指定します。

   オーサーインスタンスに接続する場合は、認証方法を更新します。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/_cq_graphql/wknd-shared/endpoint.json
   
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

1. 新しいブラウザーウィンドウが読み込まれます。 [http://localhost:3000](http://localhost:3000)
1. WKND リファレンスサイトのアドベンチャーのリストがアプリケーションに表示される必要があります。

## コード

React アプリケーションの構築方法、GraphQL の永続クエリを使用してコンテンツを取得するAEMヘッドレスへの接続方法、およびそのデータの表示方法の概要を次に示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 永続クエリ

AEMヘッドレスのベストプラクティスに従い、React アプリケーションはAEM GraphQL に永続化されたクエリを使用して、アドベンチャーデータをクエリします。 アプリケーションでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 持続的なクエリで、AEM内のすべてのアドベンチャを簡潔なプロパティセットで返します。 この永続的なクエリは、初期ビューのアドベンチャーリストを駆動します。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 次の条件を満たす 1 つのアドベンチャーを返す永続クエリ `slug` 一連のプロパティを持つ（アドベンチャーを一意に識別するカスタムプロパティ）。 この永続的なクエリは、アドベンチャーの詳細表示を強化します。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

### GraphQL 永続クエリを実行

AEMで永続化されたクエリは HTTPGETを介して実行されるので、 [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js) は [永続化された GraphQL クエリの実行](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEMに対して貼り付け、アドベンチャーコンテンツをアプリに読み込みます。

永続化された各クエリは、 `src/api/persistedQueries.js`を呼び出し、AEM HTTPGETエンドポイントを非同期的に呼び出して、アドベンチャーデータを返します。

次に、各関数が `aemHeadlessClient.runPersistedQuery(...)`に設定され、永続化された GraphQL クエリを実行します。

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### 表示

React アプリケーションは、2 つのビューを使用して、Web エクスペリエンスにアドベンチャーデータを表示します。

+ `src/components/Adventures.js`

   呼び出し `getAllAdventures()` から `src/api/persistedQueries.js`  返された冒険をリストに表示します。

+ `src/components/AdventureDetail.js`

   を呼び出します。 `getAdventureBySlug(..)` の使用 `slug` アドベンチャー選択経由で渡されるパラメーター `Adventures` コンポーネントを選択し、単一のアドベンチャーの詳細を表示します。


### 環境変数

複数 [環境変数](https://create-react-app.dev/docs/adding-custom-environment-variables) を使用してAEM環境に接続します。 デフォルトでは、次の場所で実行されている AEM パブリッシュに接続します： `http://localhost:4503`. AEM接続を変更するには、 `.env.development` ファイル：

+ `REACT_APP_HOST_URI=http://localhost:4502`:AEM target host に設定
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`:GraphQL エンドポイントパスを設定します。 このアプリは永続化されたクエリのみを使用するので、この React アプリでは使用されません。
+ `REACT_APP_AUTH_METHOD=`:推奨される認証方法。 オプション。デフォルトでは認証は使用されません。
   + `service-token`:サービス資格情報を使用してAEM as a Cloud Serviceのアクセストークンを取得する
   + `dev-token`:AEM as a Cloud Serviceのローカル開発に開発トークンを使用
   + `basic`:ローカル AEM オーサーとのローカル開発にユーザー/パスを使用
   + 認証なしでAEMに接続する場合は空白のままにします
+ `REACT_APP_AUTHORIZATION=admin:admin`:AEM オーサー環境に接続する場合に使用する基本認証資格情報を設定します（開発用のみ）。 パブリッシュ環境に接続する場合、この設定は不要です。
+ `REACT_APP_DEV_TOKEN`:開発トークン文字列。 リモートインスタンスに接続するには、Basic auth (user:pass) の横で、Cloud Console の DEV トークンと共に Bearer auth を使用できます
+ `REACT_APP_SERVICE_TOKEN`:サービス資格情報ファイルのパス。 リモートインスタンスに接続する場合は、サービストークン（開発者コンソールからファイルをダウンロード）を使用して認証もおこなえます。

### プロキシAEMリクエスト

WebPack 開発サーバーを使用する場合 (`npm start`) プロジェクトが [プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. ファイルは次の場所に設定されています。 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) とは、で設定された複数のカスタム環境変数を使用します。 `.env` および `.env.development`.

AEMオーサー環境に接続する場合、 [認証方法を設定する必要があります](#environment-variables).

### クロスオリジンリソース共有 (CORS)

この React アプリケーションは、ターゲットAEM環境で実行されるAEMベースの CORS 設定に依存し、React アプリがで実行されると想定します。 `http://localhost:3000` 開発モードで。 この [CORS 設定](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) は、 [WKND サイト](https://github.com/adobe/aem-guides-wknd).

![CORS 設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
