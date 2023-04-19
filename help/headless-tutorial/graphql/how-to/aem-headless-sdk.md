---
title: AEMヘッドレス SDK の使用
description: AEMヘッドレス SDK を使用してGraphQLクエリを実行する方法について説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 31948793786a2c430533d433ae2b9df149ec5fc0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 10%

---

# AEMヘッドレス SDK

AEMヘッドレス SDK は、クライアントが HTTP 経由でAEMヘッドレス API をすばやく簡単に操作するために使用できるライブラリのセットです。

AEMヘッドレス SDK は、次の様々なプラットフォームで使用できます。

+ [クライアントサイドブラウザー用 AEM ヘッドレス SDK（JavaScript）](https://github.com/adobe/aem-headless-client-js)
+ [サーバーサイド／Node.js（JavaScript）用 AEM ヘッドレス SDK](https://github.com/adobe/aem-headless-client-nodejs)
+ [Java™ 用 AEM ヘッドレス SDK](https://github.com/adobe/aem-headless-client-java)

## 永続的な GraphQL クエリ

永続化されたクエリを使用してGraphQLを使用してAEMに対するクエリを実行する（とは異なります） [クライアント定義GraphQLクエリ](#graphl-queries)) を使用すると、開発者はAEMでクエリを（結果ではなく）保持し、クエリの名前による実行をリクエストできます。 永続クエリは、SQL データベースのストアドプロシージャの概念と似ています。

永続化されたクエリは HTTPGETを使用して実行されます。HTTP データは CDN およびAEM Dispatcher 層でキャッシュ可能です。このデータは、クライアントが定義したGraphQLクエリよりも永続化されたクエリの方がパフォーマンスが高くなります。 また、永続化されたクエリが有効になり、API が定義され、開発者が各コンテンツフラグメントモデルの詳細を理解する必要性を切り離します。

### コードの例{#persisted-graphql-queries-code-examples}

AEMに対してGraphQLの永続クエリを実行する方法のコード例を次に示します。

+++ JavaScript の例

のインストール [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を実行することで `npm install` コマンドを Node.js プロジェクトのルートから削除します。

```
$ npm i @adobe/aem-headless-client-js
```

次のコード例は、 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を使用した npm モジュール `async/await` 構文と同じです。 JavaScript 版AEMヘッドレス SDK では、 [Promise 構文](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

このコードでは、という名前の永続的なクエリが存在すると想定しています。 `wknd/adventureNames` は AEM オーサーで作成され、AEM パブリッシュに公開されています。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(...) 例

のインストール [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を実行することで `npm install` コマンドを使用して、React プロジェクトのルートからアクセスできます。

```
$ npm i @adobe/aem-headless-client-js
```

次のコード例は、 [React useEffect(...) フック](https://reactjs.org/docs/hooks-effect.html) をクリックして、AEM GraphQLへの非同期呼び出しを実行します。

使用 `useEffect` React で非同期のGraphQL呼び出しをおこなうのは、次の理由で役立ちます。

1. AEMへの非同期呼び出し用の同期ラッパーを提供します。
1. AEMの不要な要求を減らします。

このコードでは、という名前の永続的なクエリが存在すると想定しています。 `wknd-shared/adventure-by-slug` は AEM オーサーで作成され、GraphiQL を使用して AEM パブリッシュに公開されています。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

カスタム React を呼び出す `useEffect` フックを React コンポーネント内の他の場所から

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

新規 `useEffect` フックは、React アプリが使用する永続化されたクエリごとに作成できます。

+++

<p> </p>

## GraphQLクエリ

AEMはクライアント定義のGraphQLクエリをサポートしますが、AEMのベストプラクティスは [永続的なGraphQLクエリ](#persisted-graphql-queries).

## Webpack 5+

AEMヘッドレス JS SDK は、 `util` デフォルトでは Webpack 5+に含まれていません。 Webpack 5 以降を使用している場合、次のエラーが表示されます。

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

以下を追加します。 `devDependencies` を `package.json` ファイル：

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

次に、を実行します。 `npm install` 依存関係をインストールする。
