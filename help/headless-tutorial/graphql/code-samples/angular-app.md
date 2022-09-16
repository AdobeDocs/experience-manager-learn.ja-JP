---
title: Angularアプリ
description: コンテンツフラグメントを使用してモデル化された WKND アドベンチャを表示するシンプルなAngularアプリです。
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '69'
ht-degree: 5%

---


# Angularアプリ

シンプルなAngularアプリを使用してAEMヘッドレス GraphQL API を調べます。 このAngularアプリは、 [wknd.site](https://wknd.site)は、WKND アドベンチャのリストを表示し、各アドベンチャーの詳細ビューを提供します。

このコード：

+ 接続先 [wknd.site](https://wknd.site)の AEM パブリッシュサービス。認証は不要
+ 次の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug`

