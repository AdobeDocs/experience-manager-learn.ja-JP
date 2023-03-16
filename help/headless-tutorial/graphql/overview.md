---
title: AEMヘッドレスの概要 — GraphQL
description: Experience ManagerGraphQL API とその機能について説明します。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 6%

---

# AEM ヘッドレスの概要 - GraphQL {#getting-started-with-aem-headless}

AEM GraphQL APIs for Content Fragments は、ヘッドレスな CMS シナリオをサポートし、外部クライアントアプリケーションはAEMで管理されるコンテンツを使用してエクスペリエンスをレンダリングします。

最新のコンテンツ配信 API は、JavaScript ベースのフロントエンドアプリケーションの効率とパフォーマンスを高めるための鍵となります。 REST API を使用すると、次の課題が発生します。

* 1 度に 1 つのオブジェクトを取得するためのリクエストの数が多い
* 多くの場合、「過剰な配信」コンテンツ。つまり、アプリケーションが必要以上にコンテンツを受け取ることを意味します

これらの課題を解決するために、GraphQLはクエリベースの API を提供し、クライアントは必要なコンテンツについてのみAEMに問い合わせ、1 回の API 呼び出しで受信できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

このビデオでは、AEMで実装されたGraphQL API の概要を説明します。 AEMのGraphQL API は主に、ヘッドレスデプロイメントの一環としてAEMコンテンツフラグメントをダウンストリームアプリケーションに配信するように設計されています。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM ヘッドレスの概要 - GraphQL"
>abstract="GraphQLを使用してコンテンツフラグメントを配信する方法を説明します。"
>additional-url="https://video.tv.adobe.com/v/328618/?captions=jpn" text="AEMのGraphQLの概要"

## AEMヘッドレスGraphQLビデオシリーズ

コンテンツフラグメント、AEM GraphQL API および開発ツールの詳細なウォークスルーを通じて、AEM GraphQL機能について説明します。

* [AEMヘッドレスGraphQLビデオシリーズ](./video-series/modeling-basics.md)

## AEMヘッドレスGraphQL実践チュートリアル

AEM GraphQL API を使用してコンテンツフラグメントを使用する React アプリを構築し、AEM GraphQLの機能を調べます。

* [AEMヘッドレスGraphQL実践チュートリアル](./multi-step/overview.md)

## AEM GraphQLとAEM Content Services の比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQLクエリ | AEM Page |
| 配信フォーマット | GraphQL JSON | AEM ComponentExporter JSON |
