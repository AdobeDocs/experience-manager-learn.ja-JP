---
title: React アプリのフィルタリング
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャーをフィルタリングする、シンプルな React アプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '130'
ht-degree: 100%

---

# React アプリのフィルタリング

[React](https://reactjs.org/) アプリを使用してデータをフィルタリングする AEM ヘッドレス GraphQL API 機能について詳しく説明します。この React アプリは、アクティビティタイプでフィルタリングできる WKND アドベンチャーのリストを作成します。

このコードは、Adobe の [JavaScript 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)を使用して、React から永続化された GraphQL クエリを呼び出します。このアプリは、`wknd-shared/adventures-all` 永続クエリを使用してすべてのアドベンチャーを収集し、利用可能なアクティビティタイプのリストを取得します。ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリに渡され、指定したアクティビティタイプのアドベンチャーに対してのみ、アドベンチャーの詳細を取得します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity` を使用する
