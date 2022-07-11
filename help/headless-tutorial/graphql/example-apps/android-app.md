---
title: Android アプリ — AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Android アプリケーションは、AEMの GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。
version: Cloud Service
mini-toc-levels: 2
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 8b2c116ceb6ab8c3a009dcec6629c2e97d815b7b
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 5%

---

# Android アプリ

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Android アプリケーションは、AEMの GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。 この [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) は、GraphQL クエリを実行し、データを Java オブジェクトにマッピングしてアプリを強化するために使用されます。

![AEMヘッドレスを備えた Android Java アプリ](./assets/android-java-app/android-app.png)


次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM要件

Android アプリケーションは、次のAEMデプロイメントオプションと連携します。 すべてのデプロイメントには、 [WKND サイト v2.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest) をインストールします。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances)

Android アプリケーションは、 __AEM パブリッシュ__ 環境ですが、Android アプリケーションの設定で認証が指定されている場合は、AEM オーサーからコンテンツをソース化できます。

## 使用方法

1. のクローン `adobe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 起動 [Android Studio](https://developer.android.com/studio) フォルダーを開きます。 `android-app`
1. ファイルを変更します `config.properties` 時刻 `app/src/main/assets/config.properties` および更新 `contentApi.endpoint` target AEM環境に合わせるには：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __基本認証__

   この `contentApi.user` および `contentApi.password` は、WKND GraphQL コンテンツへのアクセス権を持つローカルAEMユーザーを認証します。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. ダウンロード [Android 仮想デバイス](https://developer.android.com/studio/run/managing-avds) （API 28 以降）。
1. Android エミュレーターを使用してアプリをビルドし、デプロイします。


### AEM環境への接続

`10.0.2.2` は [特別なエイリアス IP](https://developer.android.com/studio/run/emulator-networking) （エミュレーターを使用して次を作成する場合） `10.0.2.2:4502` はと同じです。 `localhost:4502`. AEMパブリッシュ環境に接続する場合（推奨）、認証は必要なく、 `contentAPi.user` および `contentApi.password` は空白のままにすることができます。

AEMオーサー環境に接続する場合 [認証](https://github.com/adobe/aem-headless-client-java#using-authorization) が必要です。 デフォルトでは、アプリケーションは、ユーザー名とパスワードがの基本認証を使用するように設定されています。 `admin:admin`. この [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) は、 [トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). でトークンベースの認証を使用するには、クライアントビルダーを更新します。 `AdventureLoader.java` および `AdventuresLoader.java`:

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

以下に、アプリケーションの強化に使用される重要なファイルとコードの概要を示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 永続クエリ

AEMヘッドレスのベストプラクティスに従い、iOSアプリケーションはAEM GraphQL に永続化されたクエリを使用して、アドベンチャーデータをクエリします。 アプリケーションでは、次の 2 つの永続クエリを使用します。

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

AEMで永続化されたクエリは HTTPGETを介して実行されるので、 [Java 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-java) は、AEMに対して永続化された GraphQL クエリを実行し、アドベンチャーコンテンツをアプリに読み込むために使用されます。

永続化された各クエリには、AEM HTTPGETエンドポイントを非同期で呼び出し、カスタム定義を使用してアドベンチャーデータを返す、対応する「loader」クラスがあります [データモデル](#data-models).

+ `loader/AdventuresLoader.java`

   を使用して、アプリケーションのホーム画面にある Adventures のリストを取得します。 `wknd-shared/adventures-all` 永続化されたクエリ。

+ `loader/AdventureLoader.java`

   を介して選択する単一のアドベンチャーを取得します。 `slug` パラメータ、使用 `wknd-shared/adventure-by-slug` 永続化されたクエリ。

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

`Adventure.java` は、GraphQL リクエストの JSON データで初期化され、Android アプリケーションのビューで使用するアドベンチャーをモデル化する Java POJO です。

### 表示

Android アプリケーションは、2 つのビューを使用して、モバイルエクスペリエンスにアドベンチャーデータを表示します。

+ `AdventureListFragment.java`

   を呼び出します。 `AdventuresLoader` 返された冒険をリストに表示します。

+ `AdventureDetailFragment.java`

   を呼び出します。 `AdventureLoader` の使用 `slug` アドベンチャー選択経由で渡されるパラメーター `AdventureListFragment` 単一の冒険の詳細を表示し、表示します。

### リモート画像

`loader/RemoteImagesCache.java` は、Android の UI 要素で使用できるように、キャッシュ内にリモート画像を準備するのに役立つユーティリティクラスです。 アドベンチャーコンテンツは、URL を介してAEM Assets内の画像を参照し、このクラスは、そのコンテンツの表示に使用されます。

## その他のリソース

+ [AEMヘッドレスの概要 — GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)
