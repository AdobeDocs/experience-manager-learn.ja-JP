---
title: AEM ヘッドレスの概要 - GraphQL
description: Experience Manager GraphQL API とその機能について説明します。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 100%

---

# AEM ヘッドレスの基本を学ぶ - GraphQL {#getting-started-with-aem-headless}

コンテンツ フラグメント用の AEM の GraphQL API
外部クライアントアプリケーションが AEM で管理されるコンテンツを使用してエクスペリエンスをレンダリングするヘッドレス CMS シナリオをサポートします。

最新のコンテンツ配信 API は、JavaScript ベースのフロントエンドアプリケーションの効率とパフォーマンスを高めるための鍵となります。 REST API を使用すると、次の課題が発生します。

* 1 度に 1 つのオブジェクトを取得するためのリクエストの数が多い
* 多くの場合、コンテンツを「過剰配信」する（アプリケーションが必要以上に受け取る）

これらの課題を克服するために、GraphQL はクエリベースの API を提供し、クライアントが AEM に必要なコンテンツのみをクエリし、単一の API 呼び出しを使用して受信できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

このビデオでは、AEM で実装された GraphQL API の概要を説明します。 AEM の GraphQL API は主に、ヘッドレスデプロイメントの一環として AEM コンテンツフラグメントをダウンストリームアプリケーションに配信するように設計されています。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM ヘッドレスの基本を学ぶ - GraphQL"
>abstract="GraphQL を使用してコンテンツフラグメントを配信する方法を説明します。"
>additional-url="https://video.tv.adobe.com/v/328618/?captions=jpn" text="AEM の GraphQL の概要"

## AEM ヘッドレス GraphQL ビデオシリーズ

コンテンツフラグメント、AEM GraphQL API および開発ツールの詳細なチュートリアルを通じて、AEM GraphQL 機能について説明します。

* [AEM ヘッドレス GraphQL ビデオシリーズ](./video-series/modeling-basics.md)

## AEM ヘッドレス GraphQL 実践チュートリアル

AEM GraphQL API を使用してコンテンツフラグメントを使用する React アプリを構築し、AEM GraphQL の機能を調べます。

* [AEM ヘッドレス GraphQL 実践チュートリアル](./multi-step/overview.md)

## AEM GraphQL と AEM Content Services の比較

|                                | AEM GraphQL API | AEM コンテンツサービス |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM コンポーネント |
| コンテンツ | コンテンツフラグメント | AEM コンポーネント |
| コンテンツ検出 | GraphQL クエリによる | AEM ページによる |
| 配信フォーマット | GraphQL JSON | AEM ComponentExporter JSON |
