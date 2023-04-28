---
title: Android アプリ - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この Android アプリケーションは、AEM の GraphQL API を使用してコンテンツをクエリする方法の例を示しています。
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 985d52f02025dc9cb2b9c70ead4a88af07c63f29
workflow-type: ht
source-wordcount: '765'
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

Android アプリケーションは、次の AEM デプロイメントオプションと連携します。すべてのデプロイメントに [WKND Site v2.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)をインストールする必要があります。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) を使用したローカル設定
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=ja#install-local-aem-instances)

Android アプリケーションは __AEM パブリッシュ__&#x200B;環境に接続するように設計されていますが、Android アプリケーションの設定で認証が指定されている場合は、AEM オーサーからコンテンツを取得できます。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. [Android Studio](https://developer.android.com/studio) を起動し、フォルダー `android-app` を開きます。
1. `app/src/main/assets/config.properties` のファイル `config.properties` を変更し、ターゲット AEM 環境に一致するように `contentApi.endpoint` を更新します。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __基本認証__

   `contentApi.user` と `contentApi.password` は、WKND GraphQL コンテンツにアクセスできるローカル AEM ユーザーを認証します。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. [Android 仮想デバイス](https://developer.android.com/studio/run/managing-avds)（API 28 以上）をダウンロードします。
1. Android エミュレーターを使用して、アプリをビルドしデプロイします。


### AEM 環境への接続

`10.0.2.2` は、エミュレーター使用時の localhost の[特別なエイリアス IP](https://developer.android.com/studio/run/emulator-networking) で、`10.0.2.2:4502` が `localhost:4502` と同等になります。AEM パブリッシュ環境に接続する場合（推奨）、認証は必要なく、`contentAPi.user` と `contentApi.password` は空白のままで構いません。

AEM オーサー環境に接続する場合は、[認証](https://github.com/adobe/aem-headless-client-java#using-authorization)が必要です。デフォルトでは、アプリケーションはユーザー名とパスワードが `admin:admin` の基本認証を使用するように設定されています。[AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) は、[トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja)を使用する機能を提供します。トークンベースの認証を使用するには、`AdventureLoader.java` と `AdventuresLoader.java` のクライアントビルダーを更新します。

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

+ `wknd/adventures-all` 永続クエリ。AEM 内のすべてのアドベンチャーを簡潔なプロパティセットと共に返します。この永続クエリは、初期ビューのアドベンチャーリストを制御します。

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

+ `wknd/adventure-by-slug` 永続クエリ。完全なプロパティセットを持つ 1 つのアドベンチャーを、`slug`（アドベンチャーを一意に識別するカスタムプロパティ）別に返します。この永続クエリで、アドベンチャーの詳細ビューが強化されます。

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
