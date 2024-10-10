---
title: AEM ヘッドレスで大きな結果セットを扱う方法
description: AEM ヘッドレスで大きな結果セットを扱う方法を説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '843'
ht-degree: 100%

---

# AEM ヘッドレスでの大きな結果セット

AEM ヘッドレス GraphQL クエリは、大きな結果を返す場合があります。この記事では、AEM ヘッドレスで大きな結果を操作して、アプリケーションの最高のパフォーマンスを確保する方法について説明します。

AEM ヘッドレスでは、大きな結果セットの小さなサブセットに対する[オフセット／制限ベースのページネーション](#list-query)クエリと[カーソルベースのページネーション](#paginated-query)クエリをサポートしています。複数のリクエストを実行して、必要な数の結果を収集することができます。

以下の例では、この手法を実地に説明するために、結果の小さなサブセット（リクエストごとに 4 つのレコード）を使用しています。実際のアプリケーションでは、パフォーマンスを向上させるために、リクエストあたりのレコード数はもっと多くなるでしょう。リクエストあたり 50 レコードが適切なベースラインです。

## コンテンツフラグメントモデル

ページネーションと並べ替えは、任意のコンテンツフラグメントモデルに対して使用できます。

## GraphQL 永続クエリ

大きなデータセットを扱う場合は、オフセット／制限ベースのページネーションとカーソルベースのページネーションの両方を使用して、データの特定のサブセットを取得できます。ただし、この 2 つの手法には違いがいくつかあり、状況によってはどちらか一方がより適切となる場合があります。

### オフセット／制限

`limit` と `offset` を使用したリストクエリは、開始点（`offset`）と取得するレコード数（`limit`）を指定する簡単なアプローチを提供します。このアプローチでは、結果の特定のページに移動するなど、結果のサブセットを結果セット全体における任意の位置から選択できます。実装は簡単ですが、大きな結果を処理する場合は、時間がかかり非効率になる可能性があります。これは、多くのレコードを取得するには、以前のすべてのレコードをスキャンする必要があるからです。また、このアプローチでは、オフセット値が大きい場合にパフォーマンス上の問題が発生する可能性もあります。多くの結果を取得して破棄する必要が生じる場合があるからです。

#### GraphQL クエリ

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### クエリ変数

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL 応答

結果の JSON 応答には、2 番目、3 番目、4 番目および 5 番目に高価なアドベンチャーが含まれています。結果の最初の 2 つのアドベンチャーは同じ価格です（`4500` なので、[リストクエリ](#list-queries)は同じ価格のアドベンチャーを指定し、その後でタイトルの昇順に並べ替えられます）。

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### ページ分割されたクエリ

カーソルベースのページネーションは、ページ分割されたクエリで使用でき、カーソル（特定のレコードへの参照）を使用して次の結果セットを取得するものです。このアプローチは、以前のすべてのレコードをスキャンして必要なデータサブセットを取得する必要がないので、より効率的です。ページ分割されたクエリは、大きな結果セットを最初から途中まで、または最後まで反復処理するのに最適です。`limit` と `offset` を使用したリストクエリは、開始点（`offset`）と取得するレコード数（`limit`）を指定する簡単なアプローチを提供します。このアプローチでは、結果の特定のページに移動するなど、結果のサブセットを結果セット全体における任意の位置から選択できます。実装は簡単ですが、大きな結果を処理する場合は、時間がかかり非効率になる可能性があります。これは、多くのレコードを取得するには、以前のすべてのレコードをスキャンする必要があるからです。また、このアプローチでは、オフセット値が大きい場合にパフォーマンス上の問題が発生する可能性もあります。多くの結果を取得して破棄する必要が生じる場合があるからです。

#### GraphQL クエリ

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### クエリ変数

```json
{
  "first": 3
}
```

#### GraphQL 応答

結果の JSON 応答には、2 番目、3 番目、4 番目および 5 番目に高価なアドベンチャーが含まれています。結果の最初の 2 つのアドベンチャーは同じ価格です（`4500` なので、[リストクエリ ](#list-queries)は同じ価格のアドベンチャーを指定し、その後でタイトルの昇順に並べ替えられます）。

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### ページ分割された次の結果セット

次の結果セットは、`after` パラメーターと前のクエリの `endCursor` 値を使用して取得できます。取得する結果がこれ以上ない場合、`hasNextPage` は `false` です。

##### クエリ変数

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React の例

以下は、[オフセット／制限ベースのページネーション](#offset-and-limit)と[カーソルベースのページネーション](#cursor-based-pagination)のアプローチを使用する方法を示した、React の例です。通常、リクエストあたりの結果の数はもっと多くなりますが、この例では、上限を 5 件に設定しています。

### オフセット／制限ベースのページネーションの例

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

オフセット／制限ベースのページネーションを使用すると、結果のサブセットを簡単に取得して表示できます。

#### useEffect フック

`useEffect` フックは、アドベンチャーのリストを取得する永続クエリ（`adventures-by-offset-and-limit`）を呼び出します。このクエリでは、`offset` パラメーターと `limit` パラメーターを使用して、開始点と取得する結果の数を指定します。`page` 値が変更されると、`useEffect` フックが呼び出されます。


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### コンポーネント

このコンポーネントでは、`useOffsetLimitAdventures` フックを使用してアドベンチャーのリストを取得します。`page` 値を増分または減分して、次または前の結果セットを取得します。`hasMore` 値は、「次のページ」ボタンを有効にするかどうかを決定するために使用されます。

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### ページ分割の例

![ページ分割の例](./assets/large-results/paginated-example.png)

_赤いボックスはそれぞれ、ページ分割された個別の HTTP GraphQL クエリを表しています。_

カーソルベースのページネーションを使用すると、結果を増分的に収集して既存の結果に連結することで、大きな結果セットを簡単に取得して表示できます。


#### UseEffect フック

`useEffect` フックは、アドベンチャーのリストを取得する永続クエリ（`adventures-by-paginated`）を呼び出します。このクエリでは `first` パラメーターと `after` パラメーターを使用して、取得する結果の数と開始点となるカーソルを指定します。`fetchData` は継続的にループし、取得する結果がなくなるまで、ページ分割された次の結果セットを収集します。

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### コンポーネント

このコンポーネントでは、`usePaginatedAdventures` フックを使用してアドベンチャーのリストを取得します。`queryCount` 値を使用して、アドベンチャーのリストを取得するために実行された HTTP リクエストの数を表示します。

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
