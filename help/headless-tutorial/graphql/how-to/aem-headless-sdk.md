---
title: AEM ヘッドレス SDK の使用
description: AEM ヘッドレス SDK を使用して GraphQL クエリを作成する方法について説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 186
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '432'
ht-degree: 100%

---

# AEM ヘッドレス SDK

AEM ヘッドレス SDK は、クライアントが HTTP 経由で AEM ヘッドレス API を素早く簡単に操作するために使用できるライブラリのセットです。

AEM ヘッドレス SDK は、次の様々なプラットフォームで使用できます。

+ [クライアントサイドブラウザー用 AEM ヘッドレス SDK（JavaScript）](https://github.com/adobe/aem-headless-client-js)
+ [サーバーサイド／Node.js（JavaScript）用 AEM ヘッドレス SDK](https://github.com/adobe/aem-headless-client-nodejs)
+ [Java™ 用 AEM ヘッドレス SDK](https://github.com/adobe/aem-headless-client-java)

## 永続的な GraphQL クエリ

（[クライアント定義の GraphQL クエリ](#graphl-queries)ではなく）永続クエリを介した GraphQL を使用して AEM にクエリを実行すると、開発者は AEM でクエリ（ただし、その結果ではない）を永続化してから、クエリを名前で実行するようにリクエストできます。永続クエリは、SQL データベースのストアドプロシージャの概念と似ています。

永続クエリは、CDN および AEM Dispatcher 層でキャッシュ可能な HTTP GET を使用して実行されるので、クライアント定義の GraphQL クエリよりもパフォーマンスが高くなります。また、永続クエリが有効になると、API が定義され、開発者が各コンテンツフラグメントモデルの詳細を理解する必要性が切り離されます。

### コードの例{#persisted-graphql-queries-code-examples}

AEM に対して GraphQL 永続クエリを実行する方法のコード例を以下に示します。

+++ JavaScript の例

Node.js プロジェクトのルートから `npm install` コマンドを実行して、[@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) をインストールします。

```
$ npm i @adobe/aem-headless-client-js
```

このコード例は、`async/await` 構文を介して [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm モジュールを使用して AEM にクエリを実行する方法を示しています。JavaScript 用 AEM ヘッドレス SDK は、[Promise 構文](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client)もサポートしています。

このコードでは、`wknd/adventureNames` という名前の永続クエリが AEM オーサーで作成され、AEM パブリッシュに公開されていることを前提としています。

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

React プロジェクトのルートから `npm install` コマンドを実行して、[@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) をインストールします。

```
$ npm i @adobe/aem-headless-client-js
```

このコード例は、[React useEffect(..) フック](https://reactjs.org/docs/hooks-effect.html) を使用して AEM GraphQL への非同期呼び出しを実行する方法を示しています。

React で `useEffect` を使用して非同期の GraphQL 呼び出しを行うのは、次の理由で役立ちます。

1. AEM への非同期呼び出しの同期ラッパーを提供します。
1. 不必要な AEM のクエリの再実行を減らします。

このコードでは、`wknd-shared/adventure-by-slug` という名前の永続クエリが AEM オーサーで作成され、GraphiQL を使用して AEM パブリッシュに公開されていることを前提としています。

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

React コンポーネントの別の場所からカスタム React `useEffect` フックを呼び出します。

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

React アプリが使用する永続クエリごとに新しい `useEffect` フックを作成できます。

+++

<p> </p>

## GraphQL クエリ

AEM はクライアント定義の GraphQL クエリをサポートしていますが、[GraphQL 永続クエリ](#persisted-graphql-queries)を使用することが AEM のベストプラクティスです。

## Webpack 5+

AEM ヘッドレス JS SDK は `util` に依存していますが、これはデフォルトでは Webpack 5+ に含まれていません。Webpack 5+ を使用している場合、次のエラーが表示されます。

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

次の `devDependencies` を `package.json` ファイルに追加します。

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

次に、`npm install` を実行して依存関係をインストールします。
