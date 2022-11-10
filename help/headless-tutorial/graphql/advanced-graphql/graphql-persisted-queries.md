---
title: 永続的な GraphQL クエリ — AEMヘッドレスの高度な概念 — GraphQL
description: Adobe Experience Manager(AEM) ヘッドレスの高度な概念のこの章では、永続的な GraphQL クエリを作成し、パラメーターを使用して更新する方法について説明します。 永続化されたクエリでキャッシュ制御パラメーターを渡す方法を説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# 永続的な GraphQL クエリ

永続化クエリは、Adobe Experience Manager(AEM) サーバーに保存されるクエリです。 クライアントは、クエリ名を持つ HTTPGETリクエストを送信して実行できます。 このアプローチの利点は、キャッシュ可能性です。 クライアント側の GraphQL クエリは HTTPPOSTリクエストを使用して実行することもできますが、キャッシュすることはできません。永続化されたクエリは HTTP キャッシュまたは CDN を使用してキャッシュでき、パフォーマンスが向上します。 永続化されたクエリを使用すると、クエリがサーバー上にカプセル化され、AEM管理者が完全に制御できるので、リクエストを簡略化し、セキュリティを強化できます。 これは **ベストプラクティスと強く推奨** AEM GraphQL API を使用する際に持続的なクエリを使用する場合。

前の章では、WKND アプリのデータを収集するための高度な GraphQL クエリをいくつか調べました。 この章では、クエリをAEMに永続化し、永続化されたクエリに対するキャッシュ制御の使用方法を学びます。

## 前提条件 {#prerequisites}

このドキュメントは、マルチパートチュートリアルの一部です。 次の項目を [前の章](explore-graphql-api.md) は、この章の前に完了しています。

## 目的 {#objectives}

この章では、以下の方法について説明します。

* パラメーターを使用して GraphQL クエリを保持
* 永続クエリでの cache-control パラメーターの使用

## レビュー _GraphQL 永続クエリ_ 設定

私たちはそれを確認しましょう _GraphQL 永続クエリ_ は、AEMインスタンスの WKND Site プロジェクトに対して有効になっています。

1. に移動します。 **ツール** > **一般** > **設定ブラウザー**.

1. 選択 **WKND 共有**&#x200B;を選択し、「 **プロパティ** 上部のナビゲーションバーで、設定プロパティを開きます。 設定プロパティページに、 **GraphQL 永続的なクエリ** 権限が有効になっている。

   ![設定プロパティ](assets/graphql-persisted-queries/configuration-properties.png)

## 組み込みの GraphiQL Explorer ツールを使用して GraphQL クエリを保持

この節では、アドベンチャーコンテンツフラグメントデータを取得してレンダリングするために後でクライアントアプリケーションで使用される GraphQL クエリを保持します。

1. GraphiQL エクスプローラーに次のクエリを入力します。

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   クエリを保存する前に、クエリが機能することを確認します。

1. 次に「名前を付けて保存」をタップし、と入力します。 `adventure-details-by-slug` をクエリ名として使用します。

   ![GraphQL クエリを保持](assets/graphql-persisted-queries/persist-graphql-query.png)

## 特殊文字のエンコードによる変数を使用した永続クエリの実行

変数を含む永続的なクエリが、特殊文字をエンコードすることで、クライアント側のアプリケーションで実行される方法を説明します。

永続化されたクエリを実行するには、クライアントアプリケーションは次の構文を使用してGETリクエストをおこないます。

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

永続化されたクエリを実行するには _変数を使用_&#x200B;に設定すると、上記の構文は次のように変更されます。

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

セミコロン (;)、等号 (=)、スラッシュ (/)、スペースなどの特殊文字は、対応する UTF-8 エンコーディングを使用するように変換する必要があります。

を実行する `getAllAdventureDetailsBySlug` コマンドラインターミナルからクエリを実行し、これらの概念を実際に確認します。

1. GraphiQL エクスプローラーを開き、「 **省略記号** (...) 永続クエリの横 `getAllAdventureDetailsBySlug`を選択し、「 **URL をコピー**. コピーした URL をテキストパッドに貼り付けます。次のようになります。

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 追加 `yosemite-backpacking` 変数値として

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. セミコロン (;)、等号 (=) の特殊文字をエンコードします。

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. コマンドラインターミナルを開き、次を使用します。 [カール](https://curl.se/) クエリを実行

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    AEM オーサー環境に対して上記のクエリを実行する場合は、資格情報を送信する必要があります。 詳しくは、 [ローカル開発アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) それを実証し [AEM API の呼び出し](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) を参照してください。

また、 [永続化クエリの実行方法](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [クエリ変数の使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)、および [アプリで使用するクエリ URL のエンコード](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) を使用して、クライアントアプリケーションによる永続的なクエリ実行を確認します。

## 永続化されたクエリの cache-control パラメーターの更新 {#cache-control-all-adventures}

AEM GraphQL API を使用すると、パフォーマンスを向上させるために、クエリに対するデフォルトのキャッシュ制御パラメーターを更新できます。 デフォルトの cache-control 値は次のとおりです。

* 60 秒がクライアントのデフォルト (maxage=60) の TTL（例：ブラウザ）です。

* 7200 秒がデフォルトの (s-maxage=7200)Dispatcher および CDN の TTL です。「共有キャッシュ」とも呼ばれます

以下を使用： `adventures-all` クエリを使用して、cache-control パラメーターを更新します。 クエリ応答は大きく、その制御に役立ちます `age` キャッシュ内に保存されます。 この永続化されたクエリは、後で [クライアントアプリケーション](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. GraphiQL エクスプローラーを開き、「 **省略記号** (...) 永続クエリの横にある「 **ヘッダー** 開く **キャッシュ設定** モーダルです。

   ![GraphQL ヘッダーオプションを保持](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 内 **キャッシュ設定** モーダル、を更新する `max-age` ヘッダー値 `600 `秒（10 分）後に、 **保存**

   ![GraphQL キャッシュ設定を保持](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


レビュー [永続化されたクエリのキャッシュ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) デフォルトの cache-control パラメーターの詳細については、を参照してください。


## おめでとうございます。

おめでとうございます。パラメーターを使用して GraphQL クエリを永続化する方法、永続化されたクエリを更新する方法、永続化されたクエリで cache-control パラメーターを使用する方法を学習しました。

## 次の手順

内 [次の章](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)を使用する場合、WKND アプリに永続化されたクエリのリクエストを実装します。
