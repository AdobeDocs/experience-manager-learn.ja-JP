---
title: AEMのSling Model Exporterについて
description: Apache Sling Model 1.3.0では、Sling Model Exporterが導入されています。これは、Sling Modelオブジェクトをカスタムの抽象概念に書き出しまたはシリアライズする優れた方法です。 この記事では、Slingモデルを使用してHTLスクリプトを入力する従来の使用例と、Slingモデルエクスポーターフレームワークを活用してSlingモデルをJSONにシリアル化する方法を並べて説明します。
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---


# [!DNL Sling Model Exporter]を理解する

Apache [!DNL Sling Models] 1.3.0では、[!DNL Sling Model Exporter]が導入されています。これは、[!DNL Sling Model]オブジェクトをカスタムの抽象概念に書き出したり、シリアル化したりするエレガントな方法です。 この記事では、[!DNL Sling Models]を使用してHTLスクリプトを入力する従来の使用例と、[!DNL Sling Model Exporter]フレームワークを活用して[!DNL Sling Model]をJSONにシリアル化する例を並べて説明します。

## 従来のSlingモデルHTTPリクエストフロー

[!DNL Sling Models]の従来の使用例は、リソースやリクエストのビジネス抽象化を行い、HTLスクリプト（または以前のJSP）にビジネス関数にアクセスするためのインターフェイスを提供することです。

一般的なパターンは、AEMのコンポーネントまたはページを表す[!DNL Sling Models]を開発し、 [!DNL Sling Model]オブジェクトを使用してHTLスクリプトにデータを入力し、ブラウザーに表示されるHTMLの結果を生成することです。

### Sling Model HTTPリクエストフロー

![Sling Modelリクエストフロー](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEMのリソースに対してリクエストがおこなわれます。

   例: `HTTP GET /content/my-resource.html`

1. リクエストリソースの`sling:resourceType`に基づいて、適切なスクリプトが解決されます。

1. Scriptは、RequestまたはResourceを目的の[!DNL Sling Model]に適応させます。

1. スクリプトは[!DNL Sling Model]オブジェクトを使用してHTMLレンディションを生成します。

1. スクリプトによって生成されたHTMLがHTTP応答に返されます。

この従来のパターンはHTML生成のコンテキストでうまく機能します。HTLを使用すると[!DNL Sling Model]を簡単に活用できます。 JSONやXMLなどの構造化されたデータをさらに作成する方が、HTLが自然にこれらの形式の定義に役立つとは限らないので、はるかに退屈な作業です。

## [!DNL Sling Model Exporter] HTTPリクエストフロー

Apache [!DNL Sling Model Exporter]には、Slingが提供するJackson Exporterが付属し、「通常」の[!DNL Sling Model]オブジェクトをJSONに自動的にシリアル化します。 Jackson Exporterは設定可能ですが、そのコアでは[!DNL Sling Model]オブジェクトを調べ、任意の「getter」メソッドをJSONキーとして、ゲッター戻り値をJSON値として使用してJSONを生成します。

[!DNL Sling Models]の直接シリアル化により、通常のWeb要求に対して、従来の[!DNL Sling Model]要求フローを使用して作成されたHTML応答を使用して処理できます（上記を参照）。また、WebサービスやJavaScriptアプリケーションで使用できるJSONレンディションも公開します。

![Sling Model Exporter HTTPリクエストフロー](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*このフローは、提供されたJackson Exporterを使用したJSON出力の生成に関するフローを記述します。カスタムエクスポーターの使用は同じフローに従いますが、出力形式で行います。*

1. HTTPGETリクエストは、セレクターと拡張が[!DNL Sling Model]のエクスポーターに登録されたAEMのリソースに対しておこなわれます。

   例: `HTTP GET /content/my-resource.model.json`

1. Slingは、要求されたリソースの`sling:resourceType`、セレクターおよび拡張を、動的に生成されたSling Exporter Servletに解決します。このサーブレットは、エクスポーターを使用して[!DNL Sling Model]にマッピングされます。
1. 解決されたSling Exporter Servletは、要求またはリソースから適合した[!DNL Sling Model]オブジェクトに対して[!DNL Sling Model Exporter]を呼び出します（Sling Modelsアダプティブテーブルで決定）。
1. エクスポーターは、エクスポーターオプションとエクスポーター固有のSling Model注釈に基づいて[!DNL Sling Model]をシリアル化し、その結果をSling Exporter Servletに返します。
1. Sling Exporter Servletは、HTTP応答で[!DNL Sling Model]のJSONレンディションを返します。

>[!NOTE]
>
>Apache Slingプロジェクトは[!DNL Sling Models]をJSONにシリアル化するJackson Exporterを提供しますが、Exporterフレームワークはカスタムエクスポーターもサポートします。 例えば、プロジェクトで[!DNL Sling Model]をXMLにシリアル化するカスタムエクスポーターを実装できます。

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]を実行するだけでなく、Javaオブジェクトとして書き出すこともできます。 他のJavaオブジェクトへのエクスポートは、HTTPリクエストフローで役割を果たさないので、上の図には表示されません。

## サポート資料

* [ [!DNL Sling Model Exporter] ApacheFrameworkドキュメント](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
