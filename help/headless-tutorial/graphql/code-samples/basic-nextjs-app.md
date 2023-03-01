---
title: 基本的な Next.js アプリ
description: WKND アドベンチャとその詳細のリストを表示する基本的な Next.js アプリ
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 772acab316ba2ff463fa5cacff02013bea920579
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# 基本的な Next.js アプリ

この [Next.js](https://nextjs.org/) アプリでは、永続化されたクエリを使用してAEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。 このアプリケーションは、WKND Adventures のフィルタリング可能なをレンダリングし、アドベンチャーを選択すると、アドベンチャーの詳細を表示します。

このコード：

+ AEM パブリッシュサービスに接続し、認証は不要
+ WKND の永続クエリを使用します。 `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug`

この Next.js アプリの構築方法の詳細については、 [Next.js アプリのドキュメントの例](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io は、埋め込み IDE での Next.js アプリケーションの編集をサポートしていません。 このコードサンプルを編集するには、 [codesandbox.io で Next.js アプリを直接開きます。](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
