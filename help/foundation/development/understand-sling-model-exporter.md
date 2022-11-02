---
title: AEMの Sling Model Exporter について
description: Apache Sling Models 1.3.0 では、Sling Model オブジェクトをカスタムの抽象概念に書き出しまたはシリアル化する優れた方法である Sling Model Exporter が導入されました。 この記事では、Sling モデルを使用して HTL スクリプトを入力する従来の使用例を、Sling Model Exporter フレームワークを活用して Sling モデルを JSON にシリアル化する方法について説明します。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# 理解 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 を導入 [!DNL Sling Model Exporter]、エクスポートまたはシリアル化する優れた方法 [!DNL Sling Model] オブジェクトをカスタムの抽象概念に変換します。 この記事では、従来の [!DNL Sling Models] を使用して HTL スクリプトに入力するには、 [!DNL Sling Model Exporter] シリアル化するフレームワーク [!DNL Sling Model] を JSON に変換します。

## 従来の Sling モデル HTTP リクエストフロー

の従来の使用例 [!DNL Sling Models] は、リソースまたはリクエストのビジネス抽象化を提供し、HTL スクリプト（または以前の JSP）にビジネス関数にアクセスするためのインターフェイスを提供します。

一般的なパターンが開発されています [!DNL Sling Models] AEMコンポーネントまたはページを表し、 [!DNL Sling Model] オブジェクトを使用して、HTL スクリプトにデータを入力します。HTMLの結果はブラウザーに表示されます。

### Sling Model HTTP リクエストフロー

![Sling Model リクエストフロー](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEMのリソースに対してリクエストがおこなわれます。

   例：`HTTP GET /content/my-resource.html`

1. リクエストリソースの `sling:resourceType`を指定すると、適切なスクリプトが解決されます。

1. Script は、Request または Resource を目的の [!DNL Sling Model].

1. スクリプトは [!DNL Sling Model] オブジェクトを使用してHTMLレンディションを生成します。

1. スクリプトで生成されたHTMLは、HTTP 応答で返されます。

この従来のパターンは、 [!DNL Sling Model] は、HTL を使用して簡単に利用できます。 JSON や XML など、より構造化されたデータを作成する方が、HTL が自然にこれらの形式の定義に適用されないので、はるかに退屈な作業です。

## [!DNL Sling Model Exporter] HTTP リクエストフロー

Apache [!DNL Sling Model Exporter] には、「通常」を自動的にシリアル化する Sling が提供する Jackson Exporter が付属しています [!DNL Sling Model] オブジェクトを JSON に変換します。 Jackson Exporter は、設定が可能な状態で、その中核では [!DNL Sling Model] オブジェクトを生成し、JSON キーとして「getter」メソッドを使用し、JSON 値として getter 戻り値を使用して JSON を生成します。

の直接のシリアル化 [!DNL Sling Models] を使用すると、通常の Web リクエストと、従来の [!DNL Sling Model] リクエストフロー（上記を参照）を追加しますが、Web サービスや JavaScript アプリケーションで使用できる JSON レンディションも公開します。

![Sling Model Exporter HTTP リクエストフロー](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*このフローは、提供された Jackson Exporter を使用した JSON 出力の生成に関するフローを記述します。 カスタムエクスポーターの使用は、同じフローに従いますが、出力形式で行います。*

1. HTTPGETリクエストは、AEM内のリソースに対しておこなわれ、そのリソースにはセレクターと拡張子が [!DNL Sling Model]のエクスポーター。

   例：`HTTP GET /content/my-resource.model.json`

1. Sling は、リクエストされたリソースの `sling:resourceType`、セレクターおよび拡張子は、 [!DNL Sling Model] エクスポータを使用。
1. 解決された Sling Exporter Servlet は、 [!DNL Sling Model Exporter] 反対 [!DNL Sling Model] オブジェクトは、要求またはリソースから適応します（ Sling Models アダプティブテーブルで指定）。
1. エクスポーターが [!DNL Sling Model] 「Exporter Options」および「Exporter-specific Sling Model」の注釈に基づいて、結果を Sling Exporter Servlet に返します。
1. Sling Exporter Servlet が、 [!DNL Sling Model] 」と入力します。

>[!NOTE]
>
>Apache Sling プロジェクトはシリアル化する Jackson Exporter を提供していますが、 [!DNL Sling Models] JSON に変換する場合、エクスポーターフレームワークはカスタムエクスポーターもサポートします。 例えば、プロジェクトで、 [!DNL Sling Model] を XML に変換します。

>[!NOTE]
>
>次の場合にのみ実行されません。 [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models]を使用する場合は、Java オブジェクトとして書き出すこともできます。 他の Java オブジェクトへの書き出しは、HTTP リクエストフローで役割を果たさないので、上の図には表示されません。

## サポート資料

* [Apache [!DNL Sling Model Exporter] Framework のドキュメント](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
