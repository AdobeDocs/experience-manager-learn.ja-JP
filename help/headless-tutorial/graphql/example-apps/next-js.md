---
title: Next.js - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この Next.js アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行する方法を示しています。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '744'
ht-degree: 100%

---

# Next.js アプリケーション

サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この Next.js アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行する方法を示しています。JavaScript 用 AEM ヘッドレスクライアントは、アプリを強化する GraphQL の永続クエリを実行するために使用されます。

![AEM ヘッドレスと Next.js アプリケーション](./assets/next-js/next-js.png)

[GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)を表示

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

+ [Node.js v18](https://nodejs.org/ja/)
+ [Git](https://git-scm.com/)

## AEM の要件

Next.js アプリは、次の AEM デプロイメントオプションで動作します。すべてのデプロイメントで、[WKND Shared v3.0.0 以降](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)または [WKND Site v3.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)を AEM as a Cloud Service 環境にインストールする必要があります。

この例の Next.js アプリケーションは、__AEM パブリッシュ__&#x200B;サービスに接続するように設計されています。

### AEM オーサーの要件

Next.js は、__AEM パブリッシュ__&#x200B;サービスに接続し、保護されていないコンテンツにアクセスするように設計されています。Next.js は、以下で説明する `.env` プロパティを通じて AEM オーサーに接続するように設定できます。AEM オーサーから提供される画像には認証が必要なので、Next.js アプリケーションにアクセスするユーザーも AEM オーサーにログインする必要があります。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `aem-guides-wknd-graphql/next-js/.env.local` ファイルを編集し、`NEXT_PUBLIC_AEM_HOST` を AEM サービスに設定します。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   AEM オーサーサービスに接続する場合、AEM オーサーサービスはデフォルトでセキュリティ保護されているので、認証を提供する必要があります。

   ローカルの AEM アカウントセット `AEM_AUTH_METHOD=basic` を使用するには、`AEM_AUTH_USER` と `AEM_AUTH_PASSWORD` のプロパティでユーザー名とパスワードを指定します。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   [AEM as a Cloud Service ローカル開発トークン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=ja#generating-the-access-token)を使用するには、`AEM_AUTH_METHOD=dev-token` を設定し、`AEM_AUTH_DEV_TOKEN` プロパティで完全な開発トークン値を指定します。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. `aem-guides-wknd-graphql/next-js/.env.local` ファイルを編集し、`NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` が適切な AEM GraphQL エンドポイントに設定されていることを確認します。

   [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) または [WKND Site](https://github.com/adobe/aem-guides-wknd/releases/latest) を使用する場合は、`wknd-shared` GraphQL API エンドポイントを使用します。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. コマンドプロンプトを開き、次のコマンドを使用して Next.js アプリケーションを開始します。

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新規ブラウザーウィンドウが開き、[http://localhost:3000](http://localhost:3000) で Next.js アプリケーションが開きます。
1. Next.js アプリケーションにアドベンチャーが一覧表示されます。アドベンチャーを選択すると、その詳細が新しいページで開きます。

## コード

Next.js アプリケーションの構築方法、AEM ヘッドレスに接続して GraphQL 永続クエリを使用してコンテンツを取得する方法、取得したデータの表示方法の概要を次に示します。完全なコードは [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js) にあります。

### 永続クエリ

AEM ヘッドレスのベストプラクティスに従い、Next.js アプリは AEM GraphQL で永続的なクエリを使用してアドベンチャーデータのクエリを実行します。 アプリでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 持続的なクエリで、AEM 内のすべてのアドベンチャーを簡潔なプロパティセットで返します。 この永続クエリは、初期ビューのアドベンチャーリストを制御します。

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

AEM の永続化されたクエリは HTTP GET 経由で実行されるため、[JavaScript 用の AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js) は、[AEM に対して永続化された GraphQL クエリを実行](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)し、アドベンチャーコンテンツをアプリに読み込むために使用されます。

永続化された各クエリには、対応する関数が `src/lib//aem-headless-client.js` にあり、AEM GraphQL エンドポイントを呼び出して、アドベンチャーデータを返します。

各関数は順番に `aemHeadlessClient.runPersistedQuery(...)` を呼び出し、永続化された GraphQL クエリを実行します。

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### ページ

Next.js アプリは、アドベンチャーデータを提示するために 2 つのページを使用します。

+ `src/pages/index.js`

  [Next.js の getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) を使用して `getAllAdventures()` を呼び出し、各アドベンチャーをカードとして表示します。

  `getServerSiteProps()` を使用すると、この Next.js ページのサーバーサイドレンダリングが可能になります。

+ `src/pages/adventures/[...slug].js`

  1 つのアドベンチャーの詳細を表示する [Next.js Dynamic Route](https://nextjs.org/docs/routing/dynamic-routes)。この動的ルートは、`adventures/index.js` ページでのアドベンチャー選択を通じて渡された `slug` パラメーターと画像の形式、幅および画質を制御する `queryVariables` を使用した `getAdventureBySlug(slug, queryVariables)` の呼び出しを介して [Next.js の getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) を使用して、各アドベンチャーのデータをプリフェッチします。

  動的ルートは、[Next.js の getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) を使用してすべてのアドベンチャーの詳細をプリフェッチし、GraphQL クエリ `getAdventurePaths()` によって返されるアドベンチャーの完全なリストに基づいてすべての可能なルート配列を入力できます。

  `getStaticPaths()` と `getStaticProps(..)` の使用により、これらの Next.js ページの静的サイト生成が可能になりました。

## デプロイメント設定

Next.js アプリ（特にサーバーサイドレンダリング（SSR）およびサーバーサイド生成（SSG）のコンテキストでは、クロスオリジンリソース共有（CORS）などの高度なセキュリティ設定は必要ありません。

ただし、Next.js がクライアントのコンテキストから AEM に HTTP リクエストを実行する場合は、AEM のセキュリティ設定が必要な場合があります。 詳しくは、[AEM ヘッドレス単一ページアプリのデプロイメントチュートリアル](../deployment/spa.md)をご覧ください。
