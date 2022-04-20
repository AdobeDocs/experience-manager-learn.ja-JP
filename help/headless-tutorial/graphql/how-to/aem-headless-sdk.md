---
title: AEMヘッドレス SDK の使用
description: AEMヘッドレス SDK を使用して GraphQL クエリを作成する方法を説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---


# AEMヘッドレス SDK

AEMヘッドレス SDK は、クライアントが HTTP 経由でAEMヘッドレス API をすばやく簡単に操作するために使用できるライブラリのセットです。

AEMヘッドレス SDK は、次の様々なプラットフォームで使用できます。

+ [クライアント側ブラウザー用のAEMヘッドレス SDK(JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEMヘッドレス SDK for server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless SDK for Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL クエリ

GraphQL を使用したAEMのクエリ ( [永続的な GraphQL クエリ](#persisted-graphql-queries)) を使用すると、開発者は、AEMからリクエストするコンテンツを正確に指定して、コードでクエリを定義できます。

GraphQL クエリは、HTTPPOSTを使用して実行されるので、永続クエリよりもパフォーマンスが低下する傾向があります。HTTP クエリは、CDN およびAEM Dispatcher 層ではキャッシュに対応しません。

### コードの例{#graphql-queries-code-examples}

AEMに対して GraphQL クエリを実行する方法のコード例を次に示します。

+++ JavaScript の例

のインストール [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を実行することで `npm install` コマンドを Node.js プロジェクトのルートから削除します。

```
$ npm i @adobe/aem-headless-client-js
```

次のコード例は、 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を使用した npm モジュール `async/await` 構文と同じです。 JavaScript 版AEMヘッドレス SDK では、 [Promise 構文](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ React useEffect(...) example

のインストール [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を実行することで `npm install` コマンドを使用して、React プロジェクトのルートからアクセスできます。

```
$ npm i @adobe/aem-headless-client-js
```

次のコード例は、 [React useEffect(...) フック](https://reactjs.org/docs/hooks-effect.html) を使用してAEM GraphQL の非同期呼び出しを実行します。

使用 `useEffect` React での非同期の GraphQL 呼び出しは、次の理由で役立ちます。

1. AEMへの非同期呼び出し用の同期ラッパーを提供します。
1. 不要なAEMの要求を削減します。

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

を読み込んで使用する `useGraphQL` React コンポーネントのフックをクエリAEMに接続します。

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## 永続的な GraphQL クエリ

持続的なクエリを使用した GraphQL を使用したAEMのクエリ（とは対照的） [通常の GraphQL クエリ](#graphl-queries)) を使用すると、開発者はAEMでクエリを（結果ではなく）保持し、クエリの名前による実行をリクエストできます。 永続クエリは、SQL データベースのストアドプロシージャの概念と似ています。

永続化クエリは HTTPGETを使用して実行されるので、CDN およびAEM Dispatcher 層でキャッシュ可能なので、永続化クエリは、通常の GraphQL クエリよりも高いパフォーマンスを発揮する傾向があります。 また、永続化されたクエリが有効になり、API が定義され、開発者が各コンテンツフラグメントモデルの詳細を理解する必要性を切り離します。

### コードの例{#persisted-graphql-queries-code-examples}

AEMに対して GraphQL の永続クエリを実行する方法のコード例を次に示します。

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
```

+++

+++ React useEffect(...) 例

のインストール [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) を実行することで `npm install` コマンドを使用して、React プロジェクトのルートからアクセスできます。

```
$ npm i @adobe/aem-headless-client-js
```

次のコード例は、 [React useEffect(...) フック](https://reactjs.org/docs/hooks-effect.html) を使用してAEM GraphQL の非同期呼び出しを実行します。

使用 `useEffect` を使用して React で非同期の GraphQL 呼び出しをおこなうと、次の理由で役立ちます。

1. AEMへの非同期呼び出し用の同期ラッパーを提供します。
1. AEMの不要な要求を減らします。

このコードでは、という名前の永続的なクエリが存在すると想定しています。 `wknd/adventureNames` は AEM オーサーで作成され、AEM パブリッシュに公開されています。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

React コードの他の場所からこのコードを呼び出します。

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
