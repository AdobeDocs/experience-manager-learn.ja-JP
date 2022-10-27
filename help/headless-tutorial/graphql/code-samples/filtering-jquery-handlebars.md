---
title: jQuery と Handlebars を使用したフィルタリング
description: WKND Adventures をフィルタリングして表示する、jQuery と Handlebars を使用した JavaScript 実装。.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 2%

---


# jQuery と Handlebars を使用したフィルタリング

を使用する JavaScript アプリを使用してデータをフィルタリングするAEMヘッドレス GraphQL API 機能を調べる [jQuery](https://jquery.com/) および [ハンドルバー](https://handlebarsjs.com/). このアプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャのリストを作成します。

このコードは、Adobeの [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 永続化された GraphQL クエリを呼び出す。 このアプリは、 `wknd-shared/adventures-all` すべてのアドベンチャを収集し、使用可能なアクティビティタイプのリストを派生する永続的なクエリ。 ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリを実行し、指定したアクティビティタイプのアドベンチャーのみのアドベンチャーの詳細を取得します。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity`
