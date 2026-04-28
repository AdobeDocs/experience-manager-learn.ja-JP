---
title: Express アプリのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャーをフィルタリングする簡単な Express アプリ。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 100%

---

# Express アプリのフィルタリング

[Express](https://expressjs.com/) および [Pug](https://pugjs.org/) アプリを使用して、データをフィルタリングする AEM ヘッドレス GraphQL API 機能について見ていきましょう。 この Express アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャーのリストを作成します。

このコードは、Node.js ベースの JavaScript を使用して永続化された GraphQL クエリの呼び出すための、アドビの [NodeJS 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs)の使用を実証します。 このアプリは、すべてのアドベンチャーを収集し、使用可能なアクティビティタイプのリストを生成するために、 `wknd-shared/adventures-all` 永続クエリを使用します。 ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリに渡され、指定したアクティビティタイプのアドベンチャーに対してのみ、アドベンチャーの詳細を取得します。 アドベンチャーの詳細は、`wknd-shared/adventures-by-slug` 永続クエリを介して AEM から取得されます。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all`、`wknd-shared/adventures-by-activity`、`wknd-shared/adventures-by-slug` を使用する
