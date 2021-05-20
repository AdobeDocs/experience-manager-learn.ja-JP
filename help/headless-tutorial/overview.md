---
title: AEMヘッドレスチュートリアル
description: Adobe Experience ManagerをヘッドレスCMSとして使用する方法に関するチュートリアルのコレクションです。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# AEMヘッドレスチュートリアル

Adobe Experience Managerには、ヘッドレスエンドポイントを定義し、そのコンテンツをJSONとして配信する複数のオプションがあります。 実践チュートリアルを使用して、様々なオプションの使用方法を学び、何が適切かを選択します。

## AEM GraphQL APIのチュートリアル

コンテンツフラグメント用のAEM GraphQL API
は、外部クライアントアプリケーションがAEMで管理されたコンテンツを使用してエクスペリエンスをレンダリングするヘッドレスCMSシナリオをサポートします。

最新のコンテンツ配信APIは、JavaScriptベースのフロントエンドアプリケーションの効率とパフォーマンスを高めるための重要な要素です。 REST APIを使用すると、次のような課題が生じます。

* 一度に1つのオブジェクトを取得するリクエストの数が多い
* 多くの場合、「過剰配信」のコンテンツ。つまり、アプリケーションが必要以上にコンテンツを受信する

GraphQLは、これらの課題を克服するために、クライアントが必要なコンテンツに対してのみAEMをクエリし、1回のAPI呼び出しで受け取ることを可能にするクエリベースのAPIを提供します。

* AEM GraphQL APIの使用方法については、 AEM GraphQL APIの使用の手引きのチュートリアル](./graphql/overview.md)を参照してください。[

## トークンベースの認証に関するチュートリアル

AEMは、GraphQL、AEMコンテンツサービスからAssets HTTP APIへ、ヘッドレスな方法で操作できる様々なHTTPエンドポイントを公開します。 多くの場合、これらのヘッドレスな消費者は、保護されたコンテンツやアクションにアクセスするために、AEMに対する認証が必要になる場合があります。 これを容易にするために、AEMは、外部のアプリケーション、サービスまたはシステムからのHTTPリクエストのトークンベースの認証をサポートします。

* [外部アプリケーションからのCloud ServiceとしてのAEMの認証に関するチュートリアル](./authentication/overview.md)で、アクセストークンを使用してHTTP経由でAEMを認証する方法について説明します。

## AEM Content Servicesチュートリアル

AEM Content Servicesは、従来のAEM Pagesを活用してヘッドレスなREST APIエンドポイントを構成し、これらのエンドポイントで公開するコンテンツをAEMコンポーネントが定義（参照）します。

AEM Content Servicesでは、AEM SitesでWebページのオーサリングに使用するのと同じコンテンツの抽象化を使用して、これらのHTTP APIのコンテンツとスキーマを定義できます。 AEMページとAEMコンポーネントを使用すると、マーケターはあらゆるアプリケーションを強化できる柔軟なJSON APIを迅速に作成および更新できます。

* AEM Content Servicesの使用方法については、『 AEM Content Services使用の手引き』チュートリアル](./content-services/overview.md)を参照してください。[

## AEM GraphQLとAEM Content Servicesの比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQLクエリ別 | AEM Page |
| 配信形式 | GraphQL JSON | AEM ComponentExporter JSON |

## その他の役立つチュートリアル

ヘッドレス概念に関するその他のAEMチュートリアルは、次のとおりです。

* [AEM SPA エディターと Angular の使用の手引き](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor と React の使用の手引き](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)