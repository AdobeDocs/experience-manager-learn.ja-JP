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
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 6%

---


# React アプリ

サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示す React アプリケーションが提供されます。 JavaScript 版AEMヘッドレスクライアントは、アプリを強力に実行する GraphQL クエリを実行するために使用されます。

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

推奨 [WKND リファレンスサイトのCloud Service環境へのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) または [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) また、を使用することもできます。

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

`Adventures.js` - Adventures のカードリストを表示します。