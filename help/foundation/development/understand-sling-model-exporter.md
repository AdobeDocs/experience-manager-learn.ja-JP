---
title: AEMでのSlingモデルエクスポーターについて
description: Apache Slingモデル1.3.0には、Slingモデルオブジェクトをカスタムの抽象概念に書き出したり、シリアル化したりするエレガントな方法であるSlingモデルエクスポータが導入されています。 この記事では、Slingモデルを使用してHTLスクリプトを入力する従来の使用例と、Slingモデルエクスポーターのフレームワークを利用してSlingモデルをJSONにシリアル化する方法について説明します。
version: 6.3, 6.4, 6.5
sub-product: 基盤， content-services
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 1%

---


# 理解する [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0には、エレガントな方法 [!DNL Sling Model Exporter][!DNL Sling Model] であるオブジェクトを書き出したり、シリアル化したりする方法が、カスタムの抽象概念に含まれています。 この記事では、を使用してHTLスクリプトを入力する従来の使用例 [!DNL Sling Models] と、をJSONにシリアライズするためのフレー [!DNL Sling Model Exporter] ムワークを活用する方法 [!DNL Sling Model] について説明します。

## 従来のSlingモデルのHTTP要求フロー

の従来の使用例 [!DNL Sling Models] は、リソースや要求のビジネス抽象化を提供し、HTLスクリプト（または、以前のJSP）やビジネス機能にアクセスするためのインターフェイスを提供することです。

一般的なパターン [!DNL Sling Models][!DNL Sling Model] は、AEMのコンポーネントまたはページを表し、オブジェクトを使用してHTLスクリプトにデータを送る方法を使用して開発されています。その結果、ブラウザーに表示されるHTMLが生成されます。

### SlingモデルHTTP要求フロー

![Slingモデル要求フロー](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEMでリソースのリクエストが行われました。

   例: `HTTP GET /content/my-resource.html`

1. リクエストリソースに基づいて `sling:resourceType`、適切なスクリプトが解決されます。

1. スクリプトは、リクエストまたはリソースを目的に合わせて調整し [!DNL Sling Model]ます。

1. スクリプトは、この [!DNL Sling Model] オブジェクトを使用してHTMLレンダリングを生成します。

1. スクリプトによって生成されたHTMLがHTTP応答に返されます。

この従来のパターンはHTMLを生成する際に適しています。HTMLを使用すると簡単に利用でき [!DNL Sling Model] るので、 JSONやXMLなど、構造化されたデータをより多く作成するのは、HTLが自然にこれらの形式の定義に役立つとは限らないので、はるかに退屈な作業です。

## [!DNL Sling Model Exporter] HTTP要求のフロー

Apache [!DNL Sling Model Exporter] にはSlingが提供するJackson Exporterが付属し、「通常の」 [!DNL Sling Model] オブジェクトをJSONに自動的にシリアル化します。 Jackson Exporterは、極めて設定可能な状態で、そのコアで [!DNL Sling Model] オブジェクトを検査し、「getter」メソッドをJSONキーとして使用してJSONを生成し、getterの戻り値をJSON値として生成します。

のダイレクトシリアル化 [!DNL Sling Models][!DNL Sling Model] により、通常のWebリクエストと従来のリクエストフロー（上記を参照）で作成されたHTML応答の両方を扱うことができますが、WebサービスやJavaScriptアプリケーションで使用できるJSONレンディションも公開されます。

![SlingモデルエクスポーターのHTTP要求フロー](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*このフローは、提供されたJacksonエクスポーターを使用してJSON出力を生成するフローを説明します。 カスタムエクスポーターの使用は、同じフローに従いますが、出力形式が使用されます。*

1. AEM内のリソースに対して、HTTPGETリクエストが実行され、セレクターと拡張がエクスポーターに登録 [!DNL Sling Model]されます。

   例: `HTTP GET /content/my-resource.model.json`

1. Slingは、要求されたリソース `sling:resourceType`、セレクター、拡張を、エクスポーターと共にマップされる動的に生成されるSling Exporterサーブレットに解決 [!DNL Sling Model] します。
1. 解決されたSling Exporterサーブレットは、（Slingモデルのアダプティブテーブルで決定される）要求またはリソースから適合さ [!DNL Sling Model Exporter] れた [!DNL Sling Model] オブジェクトに対してを呼び出します。
1. エクスポーターは、エクスポーターオプションとエクスポーター固有のSlingモデルの注釈に [!DNL Sling Model] 基づいてシリアル化を行い、結果をSlingエクスポーターサーブレットに返します。
1. Sling Exporterサーブレットは、のJSONレンディションをHTTP応答 [!DNL Sling Model] に返します。

>[!NOTE]
>
>Apache SlingプロジェクトはJSONにシリアル化するJackson Exporterを提供しますが、エクスポーターフレームワークはカスタムエクスポーター [!DNL Sling Models] もサポートしています。 例えば、プロジェクトでXMLにシリアル化するカスタムエクスポーターを実装す [!DNL Sling Model] ることができます。

>[!NOTE]
>
>は [!DNL Sling Model Exporter] シリアル化を行うだけでなく **[!DNL Sling Models]、Javaオブジェクトとして書き出すこともできます。 他のJavaオブジェクトへのエクスポートは、HTTPリクエストフローでは役割を果たさないので、上の図には表示されません。

## サポート資料

* [ApacheFramework [!DNL Sling Model Exporter] ドキュメント](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
