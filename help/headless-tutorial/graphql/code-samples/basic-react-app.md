---
title: 基本的な React アプリ
description: WKND アドベンチャとその詳細のリストを表示する基本的な React アプリ
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---


# 基本的な React アプリ

この [React](https://reactjs.org/) アプリでは、持続クエリを使用してAEM GraphQL API を使用してコンテンツをクエリする方法を示します。 このアプリケーションは、WKND Adventures のフィルタリング可能なをレンダリングし、アドベンチャーを選択すると、アドベンチャーの詳細を表示します。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug`

この Next.js アプリの構築方法の詳細については、 [React アプリのドキュメントの例](../example-apps/react-app.md).
