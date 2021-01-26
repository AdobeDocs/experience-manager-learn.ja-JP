---
title: AEMヘッドレスチュートリアル
description: ヘッドレスCMSとしてAdobe Experience Managerを使用する方法に関するチュートリアルの集まりです。
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 4%

---


# AEMヘッドレスチュートリアル

Adobe Experience Managerには、ヘッドレスエンドポイントを定義し、そのコンテンツをJSONとして配信するための複数のオプションがあります。 実践チュートリアルを使用して、さまざまなオプションの使い方を学び、最適なオプションを選択します。

## AEM GraphQL APIのチュートリアル

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments配信は、リクエストに応じて使用できます。
> お使いのAEM用のAPIをCloud Serviceプログラムとして有効にするには、Adobeサポートにお問い合わせください。

AEM GraphQL API for Content Fragments
は、AEMで管理されたコンテンツを使用して外部クライアントアプリケーションがエクスペリエンスをレンダリングするヘッドレスCMSシナリオをサポートしています。

最新のコンテンツ配信APIは、JavaScriptベースのフロントエンドアプリケーションの効率性とパフォーマンスを高めるための重要な要素です。 REST APIを使用すると、次のような課題が生じます。

* 1回に1つのオブジェクトをフェッチする大量の要求
* 多くの場合、「過剰に配信」されるコンテンツ、つまりアプリケーションが必要以上に受信することを意味します

これらの課題を克服するために、GraphQLはクエリベースのAPIを提供し、クライアントは必要なコンテンツのみをクエリAEMに提供し、1回のAPI呼び出しで受け取ることができます。

* AEM GraphQL APIの使用方法については、「[AEM GraphQL APIの使用の手引き](./graphql/overview.md)」のチュートリアルを参照してください。

## トークンベースの認証のチュートリアル

AEMは様々なHTTPエンドポイントを公開し、GraphQL、AEM Content ServicesからAssets HTTP APIまで、ヘッドレスな方法でやり取りが可能です。 多くの場合、ヘッドレスのユーザーは、保護されたコンテンツやアクションにアクセスするためにAEMの認証が必要になる場合があります。 これを容易にするため、AEMは、外部のアプリケーション、サービス、またはシステムからのHTTP要求に対するトークンベースの認証をサポートしています。

* 「[外部アプリケーションのチュートリアル](./authentication/overview.md)でのAEMとしてのCloud Serviceの認証」のアクセストークンを使用して、AEMに対してHTTP経由で認証する方法を説明します。

## AEM Content Servicesチュートリアル

AEM Content Servicesは、従来のAEMページを利用してヘッドレスなREST APIエンドポイントを構成し、AEMコンポーネントは、これらのエンドポイントに公開するコンテンツを定義（参照）します。

AEM Content Servicesでは、AEM SitesのWebページのオーサリングに使用したのと同じコンテンツ抽象概念を使用して、これらのHTTP APIのコンテンツとスキーマを定義できます。 AEMページとAEMコンポーネントを使用すると、マーケターはあらゆるアプリケーションに電源を投入できる柔軟なJSON APIを迅速に作成および更新できます。

* AEM Content Servicesの使用方法については、「[AEM Content Services使用の手引き](./content-services/overview.md)」のチュートリアルを参照してください。

## AEM GraphQLとAEM Content Servicesの比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEMコンポーネント |
| コンテンツ | コンテンツフラグメント | AEMコンポーネント |
| コンテンツの検出 | GraphQLクエリ別 | AEMページ |
| 配信形式 | GraphQL JSON | AEM ComponentExporter JSON |

## その他の役に立つチュートリアル

ヘッドレスコンセプトに関するその他のAEMチュートリアルは以下のとおりです。

* [AEM SPA エディターと Angular の使用の手引き](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor と React の使用の手引き](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)