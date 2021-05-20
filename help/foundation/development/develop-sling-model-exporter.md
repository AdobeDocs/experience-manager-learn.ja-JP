---
title: AEMでのSling Model Exporterの開発
description: この技術的な手順では、Sling Model Exporterで使用するAEMの設定、JSONとしてレンディションするExporterフレームワークを使用した既存のSling Modelの強化、出力をさらにカスタマイズするExporterオプションとJackson注釈の使用方法について説明します。
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: API
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 3%

---


# Sling Model Exporterの開発

この技術的な手順では、Sling Model Exporterで使用するAEMの設定、JSONとしてレンディションするExporterフレームワークを使用した既存のSling Modelの強化、出力をさらにカスタマイズするExporterオプションとJackson注釈の使用方法について説明します。

Sling Model Exporterは、Sling Models v1.3.0で導入されました。この新機能を使用すると、Sling Modelに新しい注釈を追加して、モデルを別のJavaオブジェクトとして書き出す方法、またはJSONなどの別の形式にシリアル化する方法を定義できます。

Apache Slingは、Jackson JSONエクスポーターを提供し、他のWebサービスやJavaScriptアプリケーションなどのプログラムを使用するWebコンシューマーによって、SlingモデルをJSONオブジェクトとして書き出して使用する最も一般的なケースをカバーします。

## Sling Model Exporter用のAEMの設定

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、プロジェクトの機能であ [!DNL Apache Sling] り、AEM製品リリースサイクルに直接結び付けられていません。[!DNL Sling Model Exporter] は、AEM 6.3以降と互換性があります。

## [!DNL Sling Model Exporter]の使用例

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、HTL（または以前のJSP）を介したHTMLレンディションをサポートするビジネスロジックが既に含まれているSlingモデルを活用し、プログラムWebサービスやJavaScriptアプリケーションで使用するJSONと同じビジネス表現を公開するのに最適です。

## Slingモデルエクスポーターの作成

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

[!DNL Sling Model]で[!DNL Exporter]サポートを有効にするのは、Javaクラスに`@Exporter`注釈を追加するのと同じくらい簡単です。

## Sling Model Exporterオプションの適用

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、最終的にが書き出される方法を指定するために、モデルごとのエクスポーターオプションをエクスポーター実装に渡す [!DNL Sling Model] ことをサポートしています。これらのオプションは、通常、[!DNL Sling Model]の書き出し方法に「グローバル」に適用されます。これは、以下で説明するインライン注釈を使用して実行できるデータポイントごとに適用されます。

[!DNL Jackson Exporter] オプションは次のとおりです。

* [マッパー機能オプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [シリアル化機能のオプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson]注釈の適用

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

また、エクスポーター実装では、[!DNL Sling Model]クラスにインラインで適用できる注釈がサポートされ、データのエクスポート方法をより細かく制御できます。

* [[!DNL Jackson Exporter] 注釈](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## コード{#view-the-code}を表示します。

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## サポート資料{#supporting-materials}

* [[!DNL Jackson Mapper] 機能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 機能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] ドキュメント](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
