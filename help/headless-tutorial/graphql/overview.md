---
title: AEMヘッドレスの概要 — GraphQL
description: Experience ManagerGraphQL API とその機能について説明します。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 3%

---

# AEM ヘッドレスの概要 - GraphQL

AEM GraphQL APIs for Content Fragments は、外部クライアントアプリケーションがAEMで管理されるコンテンツを使用してエクスペリエンスをレンダリングするヘッドレス CMS シナリオをサポートします。

最新のコンテンツ配信 API は、JavaScript ベースのフロントエンドアプリケーションの効率とパフォーマンスを高めるための鍵となります。 REST API を使用すると、次の課題が発生します。

* 1 度に 1 つのオブジェクトを取得するためのリクエストの数が多い
* 多くの場合、「過剰な配信」コンテンツ。つまり、アプリケーションが必要以上にコンテンツを受け取ることを意味します

GraphQL は、これらの課題を解決するために、クエリベースの API を提供し、クライアントは必要なコンテンツに対してのみAEMをクエリし、1 回の API 呼び出しでを受け取ることができます。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

このビデオでは、AEMで実装された GraphQL API の概要を説明します。 AEMの GraphQL API は主に、ヘッドレスデプロイメントの一環として、AEMコンテンツフラグメントをダウンストリームアプリケーションに配信するように設計されています。

## AEMヘッドレス GraphQL ビデオシリーズ

コンテンツフラグメントとAEM GraphQL API および開発ツールの詳細なウォークスルーを通じて、AEM GraphQL 機能について説明します。

* [AEMヘッドレス GraphQL ビデオシリーズ](./video-series/modeling-basics.md)

## AEMヘッドレス GraphQL 実践チュートリアル

AEM GraphQL API を使用してコンテンツフラグメントを使用する React アプリを構築することで、AEM GraphQL 機能を調べます。

* [AEMヘッドレス GraphQL 実践チュートリアル](./multi-step/overview.md)

## AEM GraphQL とAEM Content Services の比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQL クエリ別 | AEM Page |
| 配信フォーマット | GraphQL JSON | AEM ComponentExporter JSON |
