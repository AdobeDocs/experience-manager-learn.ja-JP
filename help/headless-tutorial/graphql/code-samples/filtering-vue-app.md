---
title: Vue アプリケーションのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャをフィルタリングする、シンプルな Vue アプリケーションです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---


# Vue アプリケーションのフィルタリング

AEM ヘッドレス GraphQL API の機能を探索し、[Vue](https://vuejs.org/) アプリケーションを使用してデータをフィルタリングします。この React アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャーのリストを作成します。

このコードは、アドビの [JavaScript 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)を使用して、Vue から GraphQL 永続クエリを呼び出す方法を示しています。このアプリは、`wknd-shared/adventures-all` 永続クエリを使用してすべてのアドベンチャーを収集し、利用可能なアクティビティタイプのリストを取得します。ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリに渡され、指定したアクティビティタイプのアドベンチャーに対してのみ、アドベンチャーの詳細を取得します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity` を使用する
