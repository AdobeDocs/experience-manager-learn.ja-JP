---
title: ExpressJS および Pug アプリのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャをフィルタリングする、シンプルな ExpressJS/Pug アプリです。
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
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---


# ExpressJS および Pug アプリのフィルタリング

を使用してデータをフィルタリングするAEMヘッドレスGraphQL API 機能について詳しく見る [ExpressJS](https://expressjs.com/)/[プグ](https://pugjs.org/) アプリを使用します。 この ExpressJS/Pug アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャのリストを作成します。

このコードは、Adobeの [NodeJS 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) Node.js ベースの JavaScript を使用して永続化されたGraphQLクエリを呼び出す。 このアプリは、 `wknd-shared/adventures-all` すべてのアドベンチャを収集し、使用可能なアクティビティタイプのリストを派生する永続的なクエリ。 ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリを実行し、指定したアクティビティタイプのアドベンチャーのみのアドベンチャーの詳細を取得します。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity`
