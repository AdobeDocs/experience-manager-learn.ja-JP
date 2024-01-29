---
title: 基本の React アプリ
description: WKND アドベンチャーとその詳細のリストを表示する基本の React アプリ
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 21
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '91'
ht-degree: 100%

---

# 基本の React アプリ

この [React](https://reactjs.org/) アプリでは、永続化されたクエリを使用して、AEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。  このアプリケーションは、WKND アドベンチャーのフィルタリング可能なものをレンダリングし、アドベンチャーを選択すると、そのアドベンチャーの詳細を表示します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug` を使用します。

この Next.js アプリの構築方法の詳細については、[React アプリのドキュメントの例](../example-apps/react-app.md)をご覧ください。
