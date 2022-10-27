---
title: フィルターAngularアプリ
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャをフィルタリングするシンプルなAngularアプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# フィルターAngularアプリ

を使用してデータをフィルタリングするAEMヘッドレス GraphQL API の機能を調べる [Angular](https://angular.io/) アプリを使用します。 このAngularアプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャのリストを作成します。

このコードは、Adobeの [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) をクリックして、永続化された GraphQL クエリをAngularから呼び出します。 このアプリは、 `wknd-shared/adventures-all` すべてのアドベンチャを収集し、使用可能なアクティビティタイプのリストを派生する永続的なクエリ。 ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリを実行し、指定したアクティビティタイプのアドベンチャーのみのアドベンチャーの詳細を取得します。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity`
