---
title: Next.js - AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Next.js アプリケーションは、永続化されたクエリを使用し、AEM GraphQL API を使用してコンテンツをクエリする方法を示します。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 2%

---

# Next.js アプリ

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Next.js アプリケーションは、永続化されたクエリを使用し、AEM GraphQL API を使用してコンテンツをクエリする方法を示します。 JavaScript 版AEMヘッドレスクライアントは、アプリを強化する GraphQL の永続クエリを実行するために使用されます。

![AEMヘッドレスを備えた Next.js アプリ](./assets/next-js/next-js.png)

次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [Node.js v16 以降](https://nodejs.org/ja/)
+ [npm 8 以降](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要件

Next.js アプリケーションは、次のAEMデプロイメントオプションと連携します。 すべてのデプロイメントに必要 [WKND Shared v2.1.0 以降](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [WKND サイト v2.1.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)、または [参照デモアドオン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) をAEMas a Cloud Service環境にインストールする。

この例の Next.js アプリは、 __AEM パブリッシュ__ サービス。

### AEM オーサーの要件

Next.js は、 __AEM パブリッシュ__ 保護されていないコンテンツに対するサービスとアクセス Next.js は、 `.env` 以下に説明するプロパティ。 AEM オーサーから提供される画像には認証が必要なので、Next.js アプリにアクセスするユーザーも AEM オーサーにログインする必要があります。

## 使用方法

1. のクローン `adobe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. を編集します。 `aem-guides-wknd-graphql/next-js/.env.local` ファイルとセット `NEXT_PUBLIC_AEM_HOST` をAEMサービスに追加します。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   AEM オーサーサービスに接続する場合は、AEM オーサーサービスがデフォルトでセキュリティで保護されているので、認証を提供する必要があります。

   ローカルのAEMアカウントセットを使用するには `AEM_AUTH_METHOD=basic` をクリックし、 `AEM_AUTH_USER` および `AEM_AUTH_PASSWORD` プロパティ。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   を使用するには [AEMas a Cloud Serviceローカル開発トークン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) 設定 `AEM_AUTH_METHOD=dev-token` をクリックし、 `AEM_AUTH_DEV_TOKEN` プロパティ。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. を編集します。 `aem-guides-wknd-graphql/next-js/.env.local` ファイルと検証  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` が適切なAEM GraphQL エンドポイントに設定されている。

   を使用する場合 [WKND 共有](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) または [WKND サイト](https://github.com/adobe/aem-guides-wknd/releases/latest)、 `wknd-shared` GraphQL API エンドポイント。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   を使用する場合 [参照デモアドオン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html)、 `aem-demo-assets` GraphQL API エンドポイント。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. コマンドプロンプトを開き、次のコマンドを使用して Next.js アプリを起動します。

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新しいブラウザーウィンドウが開き、次の場所に Next.js アプリが開きます。 [http://localhost:3000](http://localhost:3000)
1. Next.js アプリは、冒険のリストを表示します。 アドベンチャーを選択すると、その詳細が新しいページで開きます。

## コード

Next.js アプリの構築方法、AEMヘッドレスに接続して GraphQL の永続クエリを使用してコンテンツを取得する方法、およびそのデータの表示方法の概要を次に示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 永続クエリ

AEMヘッドレスベストプラクティスに従い、Next.js アプリはAEM GraphQL に永続化されたクエリを使用してアドベンチャーデータをクエリします。 アプリでは、次の 2 つの永続クエリを使用します。

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

永続化された各クエリは、 `src/lib//aem-headless-client.js`を呼び出してAEM GraphQL エンドポイントを返します。

次に、各関数が `aemHeadlessClient.runPersistedQuery(...)`に設定され、永続化された GraphQL クエリを実行します。

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### ページ

Next.js アプリは、アドベンチャーデータを提示するために 2 つのページを使用します。

+ `src/pages/index.js`

   使用 [Next.js の getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 呼び出す `getAllAdventures()` そして各冒険をカードとして表示します。

   の使用 `getServerSiteProps()` を使用すると、この Next.js ページのサーバーサイドレンダリングが可能になります。

+ `src/pages/adventures/[...slug].js`

   A [Next.js Dynamic Route](https://nextjs.org/docs/routing/dynamic-routes) 1 つの冒険の詳細を示す この動的ルートは、 [Next.js の getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) ～への呼び出しを通じて `getAdventureBySlug(..)` の使用 `slug` アドベンチャー選択経由で渡されるパラメーター `adventures/index.js` ページ。

   動的ルートは、 [Next.js の getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) とは、GraphQL クエリが返す冒険の完全なリストに基づいて、可能なすべてのルート順列を生成します。  `getAdventurePaths()`

   の使用 `getStaticPaths()` および `getStaticProps(..)` では、これらの Next.js ページの静的サイト生成が許可されていました。

## デプロイメント設定

Next.js アプリ ( 特にサーバーサイドレンダリング (SSR) およびサーバーサイド生成 (SSG) のコンテキストでは、クロスオリジンリソース共有 (CORS) などの高度なセキュリティ設定は必要ありません。

ただし、Next.js がクライアントのコンテキストからAEMに HTTP リクエストを実行する場合は、AEMのセキュリティ設定が必要な場合があります。 以下を確認します。 [AEMヘッドレスシングルページアプリデプロイメントチュートリアル](../deployment/spa.md) を参照してください。
