---
title: シンプルな SvelteKit アプリ
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャを表示する単純な SvelteKit アプリです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# SvelteKit アプリのフィルタリング

を使用してデータをリストするAEMヘッドレスGraphQL API 機能を調べる [SvelteKit](https://kit.svelte.dev/) アプリを使用します。 この SvelteKit アプリは、WKND アドベンチャのリストを作成し、アドベンチャの詳細を表示するために選択できます。

このコードは、Adobeの [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) をクリックして、SvelteKit から永続化されたGraphQLクエリを呼び出します。 このアプリは、 `wknd-shared/adventures-all` すべてのアドベンチャを収集し、使用可能なアクティビティタイプのリストを派生する永続的なクエリ。 アドベンチャーの詳細は、 `wknd-shared/adventures-by-slug` 永続化されたクエリ。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug`
