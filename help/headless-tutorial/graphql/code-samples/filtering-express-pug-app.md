---
title: Express アプリのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャをフィルタリングする簡単な Express アプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Express アプリのフィルタリング

を使用してデータをフィルタリングするAEMヘッドレスGraphQL API 機能について詳しく見る [速達](https://expressjs.com/) および [プグ](https://pugjs.org/) アプリを使用します。 この Express アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャのリストを作成します。

このコードは、Adobeの [NodeJS 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) Node.js ベースの JavaScript を使用して永続化されたGraphQLクエリを呼び出す。 このアプリは、 `wknd-shared/adventures-all` すべてのアドベンチャを収集し、使用可能なアクティビティタイプのリストを派生する永続的なクエリ。 ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリを実行し、指定したアクティビティタイプのアドベンチャーのみのアドベンチャーの詳細を取得します。 アドベンチャーの詳細は、 `wknd-shared/adventures-by-slug` 永続化されたクエリ。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`、および `wknd-shared/adventures-by-slug`
