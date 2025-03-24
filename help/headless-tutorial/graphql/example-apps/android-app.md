---
title: Android アプリ - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この Android アプリケーションは、AEM の GraphQL API を使用してコンテンツをクエリする方法の例を示しています。
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 100%

---

# Android アプリ

サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この Android アプリケーションは、AEM の GraphQL API を使用してコンテンツをクエリする方法の例を示しています。 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) を使用すると、GraphQL クエリを実行し、データを Java オブジェクトにマッピングしてアプリを強化することができます。

![AEM ヘッドレスを使用した Android Java アプリ](./assets/android-java-app/android-app.png)


[GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)を表示

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM の要件

Android アプリケーションは、次の AEM デプロイメントオプションと連携します。すべてのデプロイメントに [WKND Site v3.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)をインストールする必要があります。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)

Android アプリケーションは __AEM パブリッシュ__&#x200B;環境に接続するように設計されていますが、Android アプリケーションの設定で認証が指定されている場合は、AEM オーサーからコンテンツを取得できます。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. [Android Studio](https://developer.android.com/studio)、フォルダー `android-app` の順に開く
1. `app/src/main/assets/config.properties` のファイル `config.properties` を変更し、ターゲット AEM 環境に一致するように `contentApi.endpoint` を更新します。

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __基本認証__

   `contentApi.user` と `contentApi.password` は、WKND GraphQL コンテンツにアクセスできるローカル AEM ユーザーを認証します。

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. [Android 仮想デバイス](https://developer.android.com/studio/run/managing-avds)（API 28 以上）をダウンロードします。
1. Android エミュレーターを使用して、アプリをビルドしデプロイします。


### AEM 環境への接続

AEM オーサー環境に接続する場合は、[認証](https://github.com/adobe/aem-headless-client-java#using-authorization)が必要です。[AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) は、[トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja)を使用する機能を提供します。トークンベースの認証を使用するには、`AdventureLoader.java` と `AdventuresLoader.java` のクライアントビルダーを更新します。

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## コード

アプリケーションの強化に使用される重要なファイルとコードの概要を以下に示します。完全なコードは [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app) にあります。

### 永続クエリ

AEM ヘッドレスのベストプラクティスに従って、iOS アプリケーションは AEM GraphQL 永続クエリを使用してアドベンチャーデータをクエリします。アプリケーションでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 永続クエリ。AEM のすべてのアドベンチャーを要約されたプロパティセットとともに返します。この永続クエリは、初期ビューのアドベンチャーリストを制御します。

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
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 永続クエリ。`slug`（アドベンチャーを一意に識別するカスタムプロパティ）によって、完全なプロパティセットを含む単一のアドベンチャーを返します。この永続クエリで、アドベンチャーの詳細ビューが強化されます。

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
          _dynamicUrl
          _path
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

### GraphQL 永続クエリの実行

AEM の永続クエリは HTTP GET を介して実行されるので、[AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) を使用して、AEM に対して永続 GraphQL クエリを実行し、アドベンチャーコンテンツをアプリに読み込みます。

各永続クエリには、対応する「ローダー」クラスがあり、これが AEM HTTP GET エンドポイントを非同期的に呼び出し、カスタム定義の[データモデル](#data-models)を使用してアドベンチャーデータを返します。

+ `loader/AdventuresLoader.java`

  `wknd-shared/adventures-all` 永続クエリを使用して、アドベンチャーのリストを取得しアプリケーションのホーム画面に表示します。

+ `loader/AdventureLoader.java`

  `wknd-shared/adventure-by-slug` 永続クエリを使用して、`slug` パラメーターを通じて選択した単一のアドベンチャーを取得します。

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL 応答データモデル{#data-models}

`Adventure.java` は、GraphQL リクエストからの JSON データで初期化される Java POJO であり、Android アプリケーションのビューで使用するアドベンチャーをモデル化します。

### ビュー

Android アプリケーションでは 2 つのビューを使用して、アドベンチャーデータをモバイルエクスペリエンスに表示します。

+ `AdventureListFragment.java`

  `AdventuresLoader` を呼び出し、返されたアドベンチャーをリストで表示します。

+ `AdventureDetailFragment.java`

  `AdventureListFragment` ビューで選択したアドベンチャーを通じて渡された `slug` パラメーターを使用して `AdventureLoader` を呼び出し、単一のアドベンチャーの詳細を表示します。

### リモート画像

`loader/RemoteImagesCache.java` は、Android UI 要素で使用できるようにリモート画像をキャッシュ内に準備するのに役立つユーティリティクラスです。アドベンチャーコンテンツは、URL を通じて AEM Assets 内の画像を参照し、このクラスを使用してそのコンテンツを表示します。

## その他のリソース

+ [AEM ヘッドレスの基本を学ぶ - GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja)
+ [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)
