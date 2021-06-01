---
title: AEMヘッドレスの概要 — GraphQL
description: AEM GraphQL APIと機能の概要です。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---


# AEMヘッドレスの概要 — GraphQL

コンテンツフラグメント用のAEM GraphQL API
は、外部クライアントアプリケーションがAEMで管理されたコンテンツを使用してエクスペリエンスをレンダリングするヘッドレスCMSシナリオをサポートします。

最新のコンテンツ配信APIは、JavaScriptベースのフロントエンドアプリケーションの効率とパフォーマンスを高めるための重要な要素です。 REST APIを使用すると、次のような課題が生じます。

* 一度に1つのオブジェクトを取得するリクエストの数が多い
* 多くの場合、「過剰配信」のコンテンツ。つまり、アプリケーションが必要以上にコンテンツを受信する

GraphQLは、これらの課題を克服するために、クライアントが必要なコンテンツに対してのみAEMをクエリし、1回のAPI呼び出しで受け取ることを可能にするクエリベースのAPIを提供します。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

このビデオは、AEMで実装されたGraphQL APIの概要を示します。 AEMのGraphQL APIは、主に、ヘッドレスデプロイメントの一環として、AEMコンテンツフラグメントをダウンストリームアプリケーションに配信するように設計されています。

## AEMヘッドレスGraphQLビデオシリーズ

コンテンツフラグメントとAEM GraphQL APIおよび開発ツールの詳細なウォークスルーを通じて、AEM GraphQL機能について説明します。

* [AEMヘッドレスGraphQLビデオシリーズ](./video-series/modeling-basics.md)

## AEMヘッドレスGraphQL実践チュートリアル

AEM GraphQL APIを使用してコンテンツフラグメントを使用するReactアプリを構築し、AEM GraphQL機能を調べます。

* [AEMヘッドレスGraphQL実践チュートリアル](./multi-step/overview.md)

## AEM GraphQLとAEM Content Servicesの比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQLクエリ別 | AEM Page |
| 配信形式 | GraphQL JSON | AEM ComponentExporter JSON |