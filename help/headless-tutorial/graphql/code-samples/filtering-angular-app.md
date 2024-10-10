---
title: Angular アプリのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND のアドベンチャーをフィルタリングするシンプルな Angular アプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '130'
ht-degree: 100%

---

# Angular アプリのフィルタリング

[Angular](https://angular.io/) アプリを使用してデータをフィルタリングする AEM ヘッドレス GraphQL API の機能を調べます。この Angular アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャーのリストを作成します。

このコードは、Adobe の [JavaScript 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) を使用して、永続化された GraphQL クエリを Angular から呼び出す方法を示しています。このアプリは、`wknd-shared/adventures-all` 永続クエリを使用してすべてのアドベンチャーを収集し、利用可能なアクティビティタイプのリストを取得します。ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリに渡され、指定したアクティビティタイプのアドベンチャーに対してのみ、アドベンチャーの詳細を取得します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity` を使用する
