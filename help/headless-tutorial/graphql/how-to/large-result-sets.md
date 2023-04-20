---
title: AEMヘッドレスで大きな結果セットを使用する方法
description: AEMヘッドレスを使用して大きな結果セットを操作する方法を説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
source-git-commit: 9eb706e49f12a3ebd5222e733f540db4cf2c8748
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 1%

---


# AEMヘッドレスの大きな結果セット

AEMヘッドレスGraphQLクエリは、大きな結果を返す場合があります。 この記事では、AEMヘッドレスで大きな結果を処理して、アプリケーションに最適なパフォーマンスを確保する方法について説明します。

AEMヘッドレスは、 [オフセット/制限](#list-query) および [カーソルベースのページネーション](#paginated-query) より大きな結果セットの小さなサブセットに対するクエリ。 必要な数の結果を収集するように、複数のリクエストを作成できます。

以下の例では、結果の小さなサブセット（リクエストごとに 4 つのレコード）を使用して、手法を示しています。 実際のアプリケーションでは、リクエストあたり多数のレコードを使用して、パフォーマンスを向上させます。 リクエストあたり 50 レコードが適切なベースラインです。

## コンテンツフラグメントモデル

ページネーションと並べ替えは、どのコンテンツフラグメントモデルに対しても使用できます。

## GraphQL永続クエリ

大きなデータセットを扱う場合、オフセットと制限の両方と、カーソルベースのページネーションを使用して、データの特定のサブセットを取得できます。 ただし、特定の状況で 1 つの方法を他の方法よりも適切にする場合がある 2 つの方法には、いくつかの違いがあります。

### オフセット/制限

クエリのリスト、使用 `limit` および `offset` 開始点 (`offset`) および取得するレコード数 (`limit`) をクリックします。 この方法では、結果の特定のページに移動するなど、結果セット全体の任意の場所から結果のサブセットを選択できます。 実装は簡単ですが、大きな結果を処理する場合は、時間がかかり非効率になる可能性があります。これは、多くのレコードを取得するには、以前のすべてのレコードをスキャンする必要があるからです。 また、この方法では、多くの結果を取得して破棄する必要が生じる場合があるので、オフセット値が高い場合にパフォーマンスの問題が発生する可能性があります。

#### GraphQLクエリ

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

#### GraphQL応答

結果の JSON 応答には、2 番目、3 番目、4 番目、5 番目に高価なアドベンチャーが含まれます。 結果の最初の 2 つの冒険は同じ価格 (`4500` ですから、 [リストクエリ](#list-queries) 同じ価格の冒険を昇順で並べ替えます。

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

カーソルベースのページネーション（ページ分割されたクエリで使用可能）では、カーソル（特定のレコードへの参照）を使用して次の結果セットを取得する必要があります。 この方法は、以前のすべてのレコードをスキャンして、必要なデータのサブセットを取得する必要がないので、より効率的です。 ページ分割されたクエリは、最初から途中まで、または最後まで、大きな結果セットを繰り返し処理するのに最適です。 クエリのリスト、使用 `limit` および `offset` 開始点 (`offset`) および取得するレコード数 (`limit`) をクリックします。 この方法では、結果の特定のページに移動するなど、結果セット全体の任意の場所から結果のサブセットを選択できます。 実装は簡単ですが、大きな結果を処理する場合は、時間がかかり非効率になる可能性があります。これは、多くのレコードを取得するには、以前のすべてのレコードをスキャンする必要があるからです。 また、この方法では、多くの結果を取得して破棄する必要が生じる場合があるので、オフセット値が高い場合にパフォーマンスの問題が発生する可能性があります。

#### GraphQLクエリ

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

#### GraphQL応答

結果の JSON 応答には、2 番目、3 番目、4 番目、5 番目に高価なアドベンチャーが含まれます。 結果の最初の 2 つの冒険は同じ価格 (`4500` ですから、 [リストクエリ](#list-queries) 同じ価格の冒険を昇順で並べ替えます。

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

次の結果セットは、 `after` パラメーターと `endCursor` の値を指定します。 取得する結果がない場合は、 `hasNextPage` が `false`.

##### クエリ変数

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React の例

以下に、の使用方法を示す React の例を示します。 [オフセットと制限](#offset-and-limit) および [カーソルベースのページネーション](#cursor-based-pagination) アプローチ。 通常、リクエストあたりの結果の数は多くなりますが、これらの例では、上限は 5 に設定されています。

### オフセットと制限の例

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

オフセットと制限を使用すると、結果のサブセットを簡単に取得して表示できます。

#### useEffect フック

この `useEffect` フックは、永続化されたクエリ (`adventures-by-offset-and-limit`) をクリックして、Adventures のリストを取得します。 クエリでは `offset` および `limit` パラメーターを使用して、開始点と取得する結果の数を指定します。 この `useEffect` フックは、 `page` の値が変更されます。


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

このコンポーネントは、 `useOffsetLimitAdventures` フックを使って Adventures のリストを取得します。 この `page` の値は増分され、次の結果セットと前の結果セットを取得するために減分されます。 この `hasMore` 値は、次のページボタンを有効にする必要があるかどうかを決定するために使用されます。

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

_各赤いボックスは、個別のページ分割された HTTP GraphQLクエリを表します。_

カーソルベースのページネーションを使用すると、大きな結果セットを簡単に取得および表示できます。これは、結果を増分的に収集し、既存の結果に連結することで行えます。


#### UseEffect フック

この `useEffect` フックは、永続化されたクエリ (`adventures-by-paginated`) をクリックして、Adventures のリストを取得します。 クエリでは `first` および `after` 取得する結果の数と開始するカーソルを指定するパラメーター。 `fetchData` 連続ループを行い、ページ分割された次の結果セットを収集し、取得する結果がなくなるまで続きます。

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

このコンポーネントは、 `usePaginatedAdventures` フックを使って Adventures のリストを取得します。 この `queryCount` の値は、Adventures のリストを取得するためにおこなわれた HTTP リクエストの数を表示するために使用されます。

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
