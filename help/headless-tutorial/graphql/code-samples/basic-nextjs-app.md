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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '116'
ht-degree: 100%

---

# 基本的な Next.js アプリ

この [Next.js](https://nextjs.org/) アプリでは、永続クエリを使用して AEM の GraphQL API でコンテンツをクエリする方法を示します。このアプリケーションは、WKND アドベンチャーのフィルタリング可能なものをレンダリングし、アドベンチャーを選択すると、そのアドベンチャーの詳細を表示します。

このコードは次を実行します。

+ AEM パブリッシュサービスに接続し、認証を必要としない
+ WKND の永続クエリ `wknd-shared/adventures-all` および `wknd-shared/adventures-by-slug` を使用する

この Next.js アプリの構築方法の詳細については、[サンプル Next.js アプリのドキュメント](../example-apps/next-js.md)を参照してください。

>[!IMPORTANT]
>
> codesandbox.io では、組み込み IDE での Next.js アプリケーションの編集をサポートしていません。このコードサンプルを編集するには、[codesandbox.io で Next.js アプリを直接開きます](https://codesandbox.io/s/wknd-next-js-app-u8x5f8)。
