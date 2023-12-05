---
title: jQuery と Handlebars を使用したフィルタリング
description: WKND Adventures をフィルタリングして表示する、jQuery と Handlebars を使用した JavaScript 実装。.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 43
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 57%

---

# jQuery と Handlebars を使用したフィルタリング

を使用する JavaScript アプリを使用してデータをフィルタリングするAEMヘッドレスGraphQL API 機能を調べる [jQuery](https://jquery.com/) および [Handlebars](https://handlebarsjs.com/). このアプリは、アクティビティタイプでフィルタリング可能な WKND アドベンチャのリストを作成します。

このコードは、Adobeの [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 永続化されたGraphQLクエリを呼び出す。 このアプリは、`wknd-shared/adventures-all` 永続クエリを使用してすべてのアドベンチャーを収集し、利用可能なアクティビティタイプのリストを取得します。ユーザーがアクティビティタイプを選択すると、選択したタイプが `wknd-shared/adventures-by-activity` 持続的なクエリに渡され、指定したアクティビティタイプのアドベンチャーに対してのみ、アドベンチャーの詳細を取得します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-activity` を使用する
