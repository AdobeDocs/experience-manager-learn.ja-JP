---
title: クエリのフィルタリング
description: 特定のコンテンツフラグメントを選択し、その詳細を表示できる JavaScript 実装。
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# クエリのフィルタリング

次の JavaScript と Handlebars の例は、GraphQL の結果をフィルタリングし、選択した結果を表示する方法を示しています。

このコード：

+ 接続先 [wknd.site](https://wknd.site)の AEM パブリッシュサービス。認証は不要
+ 次の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug`
