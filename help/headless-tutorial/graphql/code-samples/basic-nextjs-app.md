---
title: 基本的な Next.js アプリ
description: WKND アドベンチャーとその詳細のリストを表示する基本的な Next.js アプリ
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 100%

---

# 基本的な Next.js アプリ

この [Next.js](https://nextjs.org/) アプリでは、永続クエリを使用して AEM の GraphQL API でコンテンツをクエリする方法を示します。このアプリケーションは、WKND アドベンチャーのフィルタリング可能なものをレンダリングし、アドベンチャーを選択すると、そのアドベンチャーの詳細を表示します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug` を使用する

>[!IMPORTANT]
>
> codesandbox.io では、組み込み IDE での Next.js アプリケーションの編集をサポートしていません。このコードサンプルを編集するには、[codesandbox.io で Next.js アプリを直接開きます](https://codesandbox.io/s/wknd-next-js-app-u8x5f8)。
