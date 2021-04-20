---
title: AEMでのSlingモデルエクスポーターの開発
description: この技術的な説明では、Slingモデルエクスポータで使用するAEMの設定、エクスポータフレームワークを使用した既存のSlingモデルのJSONとしてのレンダリングの拡張、エクスポータオプションとJackson注釈を使用した出力のカスタマイズ方法について説明します。
version: 6.3, 6.4, 6.5
sub-product: 基盤， content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 3%

---


# Slingモデルエクスポーターの開発

この技術的な説明では、Slingモデルエクスポータで使用するAEMの設定、エクスポータフレームワークを使用した既存のSlingモデルのJSONとしてのレンダリングの拡張、エクスポータオプションとJackson注釈を使用した出力のカスタマイズ方法について説明します。

SlingモデルエクスポーターはSlingモデルv1.3.0で導入されました。この新機能を使用すると、Slingモデルに新しい注釈を追加して、モデルを異なるJavaオブジェクトとしてエクスポートする方法や、より一般的に、JSONなどの異なる形式にシリアル化する方法を定義できます。

Apache SlingはJackson JSONエクスポーターを提供しており、SlingモデルをJSONオブジェクトとしてエクスポートして使用する最も一般的なケースをカバーしています。この方法は、他のWebサービスやJavaScriptアプリケーションなどのプログラム的なWebコンシューマーを通して行います。

## Slingモデルエクスポータ用のAEMの設定

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、 [!DNL Apache Sling] プロジェクトの機能で、AEM製品のリリースサイクルに直接結び付けられていません。[!DNL Sling Model Exporter] はAEM 6.3以降と互換性があります。

## [!DNL Sling Model Exporter]の使用例

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、HTL（または以前のJSP）を介したHTMLレンディションをサポートするビジネスロジックが既に含まれているSlingモデルを活用し、同じビジネス表現をJSONとして公開して、プログラム的なWebサービスやJavaScriptアプリケーションで使用できます。

## Slingモデルエクスポータの作成

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

[!DNL Sling Model]で[!DNL Exporter]サポートを有効にするのは、`@Exporter`注釈をJavaクラスに追加するのと同じくらい簡単です。

## Slingモデルエクスポータのオプションの適用

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、モデルごとのエクスポーターオプションをエクスポーター実装に渡して、最終的にエクスポーターがどのようにエクスポートされ [!DNL Sling Model] るかを決定できます。これらのオプションは、通常、[!DNL Sling Model]の書き出し方法に「グローバル」に適用されます。以下に説明するインライン注釈を使用して行うことができるデータポイントごとに適用されます。

[!DNL Jackson Exporter] 次のオプションがあります。

* [マッパー機能のオプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [シリアル化機能のオプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson]注釈の適用

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

また、[!DNL Sling Model]クラスにインラインで適用できる注釈をエクスポーター実装でサポートしている場合もあります。これにより、データのエクスポート方法をより詳細に制御できます。

* [[!DNL Jackson Exporter] 注釈](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## コード{#view-the-code}の表示

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## サポート資料{#supporting-materials}

* [[!DNL Jackson Mapper] 機能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 機能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] ドキュメント](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
