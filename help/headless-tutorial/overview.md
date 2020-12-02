---
title: AEMヘッドレスチュートリアル
description: ヘッドレスCMSとしてAdobe Experience Managerを使用する方法に関するチュートリアルの集まりです。
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# AEMヘッドレスチュートリアル

Adobe Experience Managerには、ヘッドレスエンドポイントを定義し、そのコンテンツをJSONとして配信するための複数のオプションがあります。 実践チュートリアルを使用して、さまざまなオプションの使い方を学び、最適なオプションを選択します。

## AEM GraphQL APIのチュートリアル

>[!CAUTION]
>
> AEM GraphQL API for Content Fragment配信は、2021年の初めにリリースされます。
> 関連ドキュメントは、プレビュー目的で利用できます。

AEM GraphQL API for Content Fragments
は、AEMで管理されたコンテンツを使用して外部クライアントアプリケーションがエクスペリエンスをレンダリングするヘッドレスCMSシナリオをサポートしています。

最新のコンテンツ配信APIは、JavaScriptベースのフロントエンドアプリケーションの効率性とパフォーマンスを高めるための重要な要素です。 REST APIを使用すると、次のような課題が生じます。

* 1回に1つのオブジェクトをフェッチする大量の要求
* 多くの場合、「過剰に配信」されるコンテンツ、つまりアプリケーションが必要以上に受信することを意味します

これらの課題を克服するために、GraphQLはクエリベースのAPIを提供し、クライアントは必要なコンテンツのみをクエリAEMに提供し、1回のAPI呼び出しで受け取ることができます。

* AEM GraphQL APIの使い方を学ぶには、「AEM GraphQL APIの使い始めに」チュートリアル[を参照してください。](./graphql/overview.md)

## AEM Content Servicesチュートリアル

AEM Content Servicesは、従来のAEMページを利用してヘッドレスなREST APIエンドポイントを構成し、AEMコンポーネントは、これらのエンドポイントに公開するコンテンツを定義（参照）します。

AEM Content Servicesでは、AEM SitesのWebページのオーサリングに使用したのと同じコンテンツ抽象概念を使用して、これらのHTTP APIのコンテンツとスキーマを定義できます。 AEMページとAEMコンポーネントを使用すると、マーケターはあらゆるアプリケーションに電源を投入できる柔軟なJSON APIを迅速に作成および更新できます。

* AEM Content Servicesの使用方法について学ぶには、「[AEM Content Services使用の手引き」チュートリアル](./content-services/overview.md)を参照してください。

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