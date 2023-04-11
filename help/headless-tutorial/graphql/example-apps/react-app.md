---
title: React アプリ — AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この React アプリケーションは、永続化されたクエリを使用してAEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 6%

---

# React アプリ{#react-app}

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この React アプリケーションは、永続化されたクエリを使用してAEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。 AEM Headless Client for JavaScript は、アプリを強化するGraphQLの永続クエリを実行するために使用されます。

![AEMヘッドレスを使用した React アプリ](./assets/react-app/react-app.png)

次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [詳細な手順のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja) この React アプリのビルドを使用する方法を説明します。

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v18](https://nodejs.org/ja/)
+ [Git](https://git-scm.com/)

## AEM要件

React アプリケーションは、次のAEMデプロイメントオプションと連携します。 すべてのデプロイメントには、 [WKND サイト v2.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) をインストールします。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances)

React アプリケーションは、 __AEM パブリッシュ__ 環境で使用できますが、React アプリケーションの設定で認証が提供されている場合は、AEM オーサーからコンテンツをソース化できます。

## 使用方法

1. のクローン `adobe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. を編集します。 `aem-guides-wknd-graphql/react-app/.env.development` ファイルとセット `REACT_APP_HOST_URI` をクリックして、ターゲットAEMを指定します。

   オーサーインスタンスに接続する場合は、認証方法を更新します。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
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

GraphQLでの永続化されたクエリを使用してコンテンツを取得するために React アプリケーションが構築される方法、AEMヘッドレスに接続する方法、およびそのデータの表示方法の概要を次に示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 永続クエリ

AEMヘッドレスのベストプラクティスに従い、React アプリケーションはAEM GraphQLで永続的なクエリを使用してアドベンチャーデータをクエリします。 アプリケーションでは、次の 2 つの永続クエリを使用します。

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

### GraphQL永続クエリの実行

AEMで永続化されたクエリは HTTPGETを介して実行されるので、 [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js) は [永続的なGraphQLクエリの実行](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEMに対して貼り付け、アドベンチャーコンテンツをアプリに読み込みます。

永続化された各クエリには、対応する React が含まれます [useEffect](https://reactjs.org/docs/hooks-effect.html) 引っ掛かる `src/api/usePersistedQueries.js`を呼び出し、AEM HTTPGET持続クエリエンドポイントを非同期的に呼び出して、アドベンチャーデータを返します。

次に、各関数が `aemHeadlessClient.runPersistedQuery(...)`に設定し、永続化されたGraphQLクエリを実行します。

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
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

### 表示

React アプリケーションは、2 つのビューを使用して、Web エクスペリエンスにアドベンチャーデータを表示します。

+ `src/components/Adventures.js`

   呼び出し `getAdventuresByActivity(..)` から `src/api/usePersistedQueries.js` 返された冒険をリストに表示します。

+ `src/components/AdventureDetail.js`

   を呼び出します。 `getAdventureBySlug(..)` の使用 `slug` アドベンチャー選択経由で渡されるパラメーター `Adventures` コンポーネントを選択し、単一のアドベンチャーの詳細を表示します。

### 環境変数

複数 [環境変数](https://create-react-app.dev/docs/adding-custom-environment-variables) を使用してAEM環境に接続します。 デフォルトでは、次の場所で実行されている AEM パブリッシュに接続します： `http://localhost:4503`. を更新します。 `.env.development` ファイルを編集し、AEM接続を変更します。

+ `REACT_APP_HOST_URI=http://localhost:4502`:AEM target host に設定
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`:GraphQLエンドポイントのパスを設定します。 このアプリは永続化されたクエリのみを使用するので、この React アプリでは使用されません。
+ `REACT_APP_AUTH_METHOD=`:推奨される認証方法。 オプション。デフォルトでは認証は使用されません。
   + `service-token`:サービス資格情報を使用してAEM as a Cloud Serviceのアクセストークンを取得する
   + `dev-token`:AEM as a Cloud Serviceのローカル開発に開発トークンを使用
   + `basic`:ローカル AEM オーサーとのローカル開発にユーザー/パスを使用
   + 認証なしでAEMに接続する場合は空白のままにします
+ `REACT_APP_AUTHORIZATION=admin:admin`:AEM オーサー環境に接続する場合に使用する基本認証資格情報を設定します（開発用のみ）。 パブリッシュ環境に接続する場合、この設定は不要です。
+ `REACT_APP_DEV_TOKEN`:開発トークン文字列。 リモートインスタンスに接続するには、基本認証 (user:pass) の横で、Cloud Console の DEV トークンで Bearer 認証を使用できます
+ `REACT_APP_SERVICE_TOKEN`:サービス資格情報ファイルのパス。 リモートインスタンスに接続する場合は、サービストークン（開発者コンソールからファイルをダウンロード）を使用して認証もおこなえます。

### プロキシAEMリクエスト

WebPack 開発サーバーを使用する場合 (`npm start`) プロジェクトが [プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. ファイルは次の場所に設定されています。 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) とは、で設定された複数のカスタム環境変数を使用します。 `.env` および `.env.development`.

AEMオーサー環境に接続する場合、 [認証方法を設定する必要があります](#environment-variables).

### クロスオリジンリソース共有 (CORS)

この React アプリケーションは、ターゲットAEM環境で実行されるAEMベースの CORS 設定に依存し、React アプリがで実行されると想定します。 `http://localhost:3000` 開発モードで。 この [CORS 設定](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) は、 [WKND サイト](https://github.com/adobe/aem-guides-wknd).

![CORS 設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
