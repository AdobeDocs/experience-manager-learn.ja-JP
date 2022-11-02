---
title: AEMでの Sling Model Exporter の開発
description: この技術的な手順では、Sling Model Exporter で使用するAEMの設定、Exporter フレームワークを使用した既存の Sling Model の JSON としてのレンディションの強化、Exporter オプションと Jackson 注釈を使用した出力のさらなるカスタマイズ方法について説明します。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 3%

---

# Sling Model Exporter の開発

この技術的な手順では、Sling Model Exporter で使用するAEMの設定、Exporter フレームワークを使用した既存の Sling Model の JSON としてのレンディションの強化、Exporter オプションと Jackson 注釈を使用した出力のさらなるカスタマイズ方法について説明します。

Sling Model Exporter は、Sling Models v1.3.0 で導入されました。この新機能を使用すると、Sling Model に新しい注釈を追加して、モデルを別の Java オブジェクトとして書き出す方法、または JSON などの別の形式にシリアル化する方法を定義できます。

Apache Sling は Jackson JSON エクスポーターを提供し、他の Web サービスや JavaScript アプリケーションなどのプログラムによる Web コンシューマーが使用する Sling モデルを JSON オブジェクトとして書き出す最も一般的な例を取り上げます。

## Sling Model Exporter 用のAEMの設定

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、 [!DNL Apache Sling] プロジェクトに直接結び付けられず、AEM製品リリースサイクルに直接結び付けられるわけではありません。 [!DNL Sling Model Exporter] は、AEM 6.3 以降と互換性があります。

## の使用例 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] は、HTL（または以前の JSP）を介したHTMLレンディションをサポートするビジネスロジックが既に含まれている Sling モデルの活用に最適で、プログラムの Web サービスや JavaScript アプリケーションで使用する場合に JSON と同じビジネス表現を公開します。

## Sling モデルエクスポーターの作成

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

有効化 [!DNL Exporter] 支持する [!DNL Sling Model] これは、 `@Exporter` 注釈を Java クラスに追加します。

## Sling Model Exporter オプションの適用

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] では、モデルごとのエクスポーターオプションをエクスポーター実装に渡して、 [!DNL Sling Model] が最後に書き出されます。 これらのオプションは、通常、 [!DNL Sling Model] は書き出されます。以下に説明するインライン注釈を介して実行できるデータポイントごとに書き出されます。

[!DNL Jackson Exporter] オプションは次のとおりです。

* [マッパー機能オプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [シリアル化機能のオプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 適用中 [!DNL Jackson] 注釈

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

また、エクスポーター実装では、 [!DNL Sling Model] クラスを使用して、データの書き出し方法をより細かく制御できます。

* [[!DNL Jackson Exporter] 注釈](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## コードを表示する {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## サポート資料 {#supporting-materials}

* [[!DNL Jackson Mapper] 機能 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 機能 Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] ドキュメント](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
