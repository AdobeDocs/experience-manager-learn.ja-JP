---
title: Android アプリ — AEMヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示す Android アプリケーションが提供されます。 Apollo Client Android は、GraphQL クエリを生成し、データを Swift オブジェクトにマッピングしてアプリを強化するために使用されます。 SwiftUI は、コンテンツの単純なリスト表示と詳細表示をレンダリングするために使用されます。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 3%

---


# Android SwiftUI アプリ

サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示す Android アプリケーションが提供されます。 10. [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) は、GraphQL クエリを実行し、データを Java オブジェクトにマッピングしてアプリを強化するために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## 前提条件 {#prerequisites}

次のツールは、ローカルにインストールする必要があります。

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## AEM要件

アプリケーションは、AEMに接続するように設計されています **公開** 最新リリースの [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest) インストール済み

* [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=ja)

推奨 [WKND リファレンスサイトのCloud Service環境へのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) または [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) また、を使用することもできます。

## 使用方法

1. を `aem-guides-wknd-graphql` リポジトリ：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) フォルダを開きます。 `android-app`
1. ファイルの変更 `config.properties` at `app/src/main/assets/config.properties` およびの更新 `contentApi.endpoint` ターゲットAEM環境に合わせるには：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. ダウンロード [Android 仮想デバイス](https://developer.android.com/studio/run/managing-avds) （最小 API 28）
1. Android エミュレーターを使用してアプリをビルドし、デプロイします。


### AEM環境への接続

`10.0.2.2` は [特別別名](https://developer.android.com/studio/run/emulator-networking) （エミュレーターを使用する場合） So `10.0.2.2:4502` はと同じです。 `localhost:4502`. AEMパブリッシュ環境に接続する場合（推奨）、認証は必要なく、 `contentAPi.user` および `contentApi.password` は空白のままにできます。

AEMオーサー環境に接続する場合 [認証](https://github.com/adobe/aem-headless-client-java#using-authorization) が必要です。 デフォルトでは、アプリケーションは、ユーザー名とパスワードがの基本認証を使用するように設定されています。 `admin:admin`. 10. [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) は、 [トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). でトークンベースの認証を使用するには、Update Client Builder を `AdventureLoader.java` および `AdventuresLoader.java`:

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

以下に、アプリケーションの強化に使用する重要なファイルとコードの概要を示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### コンテンツの取得

10. [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) は、アプリでAEMに対して GraphQL クエリを実行し、アドベンチャーコンテンツをアプリに読み込むために使用されます。

`AdventuresLoader.java` は、アプリケーションのホーム画面で Adventures の初期リストを取得して読み込むファイルです。 これは、 [永続クエリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) これは [プリパッケージ](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) と WKND リファレンスサイトの両方に表示されます。 エンドポイントは `/wknd/adventures-all`. `AEMHeadlessClientBuilder` で設定された api エンドポイントに基づいて、新しいインスタンスをインスタンス化します。 `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` は、各詳細ビューのアドベンチャーコンテンツを取得して読み込むファイルです。 再び `AEMHeadlessClient` は、クエリの実行に使用されます。 通常の graphQL クエリは、アドベンチャーコンテンツフラグメントへのパスに基づいて実行されます。 クエリは、 [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` は、GraphQL 要求の JSON データで初期化される POJO です。

`RemoteImagesCache.java` は、Android の UI 要素で使用できるように、キャッシュ内にリモート画像を準備するのに役立つユーティリティクラスです。 アドベンチャーコンテンツは、URL を介してAEM Assets内の画像を参照し、このクラスを使用してそのコンテンツを表示します。

### 表示

`AdventureListFragment.java`  — を呼び出すと、トリガー `AdventuresLoader` 返された冒険をリストに表示します。

`AdventureDetailFragment.java`  — 初期化 `AdventureLoader` そして一つの冒険の詳細を表示する

## その他のリソース

* [AEMヘッドレスの概要 — GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)

