---
title: シンプルな SvelteKit アプリ
description: コンテンツフラグメントを使用してモデル化された WKND のアドベンチャーを表示する、シンプルな SvelteKit アプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 100%

---

# SvelteKit アプリのフィルタリング

[SvelteKit](https://kit.svelte.dev/) アプリを使用してデータを一覧表示する、AEM ヘッドレス GraphQL API の機能を調べます。この SvelteKit アプリは、アドベンチャーの詳細を表示するために選択できる、WKND アドベンチャーのリストを作成します。

このコードは、アドビの [JavaScript 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)を使用して、SvelteKit から永続化された GraphQL クエリを呼び出す方法を示しています。このアプリは、`wknd-shared/adventures-all` 永続クエリを使用してすべてのアドベンチャーを収集し、利用可能なアクティビティタイプのリストを取得します。アドベンチャーの詳細は、`wknd-shared/adventures-by-slug` 永続クエリを介して要求されます。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug` を使用する
