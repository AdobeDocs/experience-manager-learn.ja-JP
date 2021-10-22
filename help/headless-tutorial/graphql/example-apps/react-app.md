---
title: React アプリ — AEMヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示す React アプリケーションが提供されます。 JavaScript 版AEMヘッドレスクライアントは、アプリを強力に実行する GraphQL クエリを実行するために使用されます。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 5%

---


# React アプリ

サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示す React アプリケーションが提供されます。 JavaScript 版AEMヘッドレスクライアントは、アプリを強力に実行する GraphQL クエリを実行するために使用されます。

表示 [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

![React アプリケーション](./assets/react-screenshot.png)

詳細な手順のチュートリアルを利用できます [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja).

## 前提条件 {#prerequisites}

次のツールは、ローカルにインストールする必要があります。

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+1%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10 以降](https://nodejs.org/ja/)
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM要件

アプリケーションは、AEMに接続するように設計されています **作成者** または **公開** 最新リリースの [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest) インストール済み

* [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=ja)

推奨 [WKND リファレンスサイトのCloud Service環境へのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) または [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances) また、を使用することもできます。

## 使用方法

1. を `aem-guides-wknd-graphql` リポジトリ：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編集 `aem-guides-wknd/react-app/.env.development` ファイルを作成し、 `REACT_APP_HOST_URI` がターゲットAEMインスタンスを指している。 認証方法を更新します（オーサーインスタンスに接続する場合）。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. ターミナルを開き、次のコマンドを実行します。

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. 新しいブラウザーウィンドウがに読み込まれます。 [http://localhost:3000](http://localhost:3000)
1. WKND リファレンスサイトからのアドベンチャーのリストがアプリケーションに表示されます。

## コード

以下に、アプリケーションの強化に使用する重要なファイルとコードの概要を示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM Headless Client for JavaScript

10. [AEM Headless Client](https://github.com/adobe/aem-headless-client-js) は、GraphQL クエリの実行に使用されます。 AEMヘッドレスクライアントは、クエリを実行する 2 つの方法を提供します。 [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) および [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` は、AEMコンテンツに対して標準の GraphQL クエリを実行します。これは、最も一般的なタイプのクエリ実行です。

[永続クエリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) は、AEMの機能で、GraphQL クエリの結果をキャッシュし、結果をGET経由で使用できるようにします。 永続化クエリは、繰り返し実行される一般的なクエリに使用します。 このアプリケーションでは、Adventures のリストは、ホーム画面で最初に実行されるクエリです。 これは非常に人気の高いクエリなので、持続的なクエリを使用する必要があります。 `runPersistedQuery` は、永続化されたクエリエンドポイントに対してリクエストを実行します。

`src/api/useGraphQL.js` は [React エフェクトフック](https://reactjs.org/docs/hooks-overview.html#effect-hook) は、パラメーターの変更をリッスンします。 `query` および `path`. If `query` が空白の場合、 `path`. ここでは、AEMヘッドレスクライアントが構築され、データの取得に使用されます。

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### アドベンチャーコンテンツ

主に Adventures のリストが表示され、アドベンチャーの詳細をクリックするオプションがユーザーに提供されます。

`Adventures.js` - Adventures のカードリストを表示します。  初期状態では、 [永続クエリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) これは [プリパッケージ](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) と WKND リファレンスサイトの両方に表示されます。 エンドポイントは `/wknd/adventures-all`. ユーザーがアクティビティに基づいて結果をフィルタリングして実験できるボタンはいくつかあります。

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
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
  `;
}
```

`AdventureDetail.js`  — アドベンチャーの詳細ビューを表示します。 URL から解析されるアドベンチャーへのパスに基づいて、graphQL クエリを作成します。

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### 環境変数

複数 [環境変数](https://create-react-app.dev/docs/adding-custom-environment-variables) は、このプロジェクトでAEM環境に接続するために使用されます。 デフォルトでは、http://localhost:4502で実行されているAEMオーサー環境に接続します。 この動作を変更する場合は、 `.env.development` ファイルの内容に応じて、

* `REACT_APP_HOST_URI=http://localhost:4502` - AEMターゲットホストに設定
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - GraphQL エンドポイントパスの設定
* `REACT_APP_AUTH_METHOD=`  — 推奨される認証方法。 オプション。デフォルトでは認証は使用されません。
   * `service-token`  — クラウド環境 PROD 用のサービストークン交換を使用
   * `dev-token` - Cloud Env でのローカル開発に開発トークンを使用
   * `basic`  — ローカルオーサー環境でのローカル開発に user/pass を使用
   * 認証方法を使用しない場合は空白のままにします。
* `REACT_APP_AUTHORIZATION=admin:admin` - AEM オーサー環境に接続する場合に使用する基本認証資格情報を設定します（開発用のみ）。 パブリッシュ環境に接続する場合、この設定は不要です。
* `REACT_APP_DEV_TOKEN`  — 開発トークン文字列。 リモートインスタンスに接続するには、Basic auth (user:pass) の横で、Cloud Console の DEV トークンで Bearer 認証を使用できます
* `REACT_APP_SERVICE_TOKEN`  — サービストークンファイルのパス。 リモートインスタンスに接続する場合は、サービストークンでも認証をおこなうことができます（クラウドコンソールからファイルをダウンロード）。

### プロキシ API リクエスト

Webpack 開発サーバーを使用する場合 (`npm start`) プロジェクトが [プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. このファイルは、 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) とは、に設定された複数のカスタム環境変数を使用します。 `.env` および `.env.development`.

AEMオーサー環境に接続する場合は、対応する認証方法を設定する必要があります。

### CORS — クロスオリジンリソース共有

このプロジェクトは、ターゲットAEM環境で実行されている CORS 設定に依存しており、アプリが開発モードでhttp://localhost:3000上で実行されていることを前提としています。 10. [COR の設定](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) は [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd).

![CORS の設定](assets/cross-origin-resource-sharing-configuration.png)

*オーサー環境の CORS 設定例*