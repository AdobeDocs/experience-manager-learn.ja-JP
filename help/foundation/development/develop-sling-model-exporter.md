---
title: AEM での Sling Model Exporter の開発
description: この技術的な手順では、Sling Model Exporter で使用する AEM の設定、Exporter フレームワークを使用した既存の Sling Model の JSONとしてのレンダリング、出力をさらにカスタマイズする Exporter オプションと Jackson 注釈の使用方法について説明します。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '363'
ht-degree: 100%

---

# Sling Model Exporter の開発

この技術的な手順では、Sling Model Exporter で使用する AEM の設定、Exporter フレームワークを使用した既存の Sling Model の JSONとしてのレンダリング、出力をさらにカスタマイズする Exporter オプションと Jackson 注釈の使用方法について説明します。

Sling Model Exporter は、Sling Models v1.3.0 で導入されました。この新機能を使用すると、Sling Model に新しい注釈を追加して、モデルを別の Java オブジェクトとして書き出す方法、JSON などの別の形式にシリアル化する方法を定義できます。

Apache Sling は Jackson JSON エクスポーターを提供し、他の web サービスや JavaScript アプリケーションなどのプログラムによる web コンシューマーが使用する Sling モデルを JSON オブジェクトとして書き出す最も一般的な例を取り上げます。

## Sling Model Exporter 用の AEM の設定

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] は [!DNL Apache Sling] プロジェクトの機能であり、AEM 製品リリースサイクルに直接結び付けられるものではありません。[!DNL Sling Model Exporter] は、AEM 6.3 以降と互換性があります。

## [!DNL Sling Model Exporter] のユースケース

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] は、HTL（または以前の JSP）を介した HTML レンディションをサポートするビジネスロジックが既に含まれている Sling モデルを活用し、同じビジネス表現を JSON として公開して、プログラムによる web サービスや JavaScript アプリケーションで利用するのに最適なものです。

## Sling Model Exporter の作成

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

[!DNL Sling Model] で [!DNL Exporter] サポートを有効にするには、Java クラスに `@Exporter` 注釈を追加するのと同じくらい容易です。

## Sling Model Exporter オプションの適用

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] はモデルごとの Exporter オプションを Exporter の実装に渡して、[!DNL Sling Model] が最終的に書き出される方法をサポートします。これらのオプションは一般に [!DNL Sling Model] がどのように書き出されるのか「グローバル」に適用され、後述のインライン注釈によって行うことができるデータポイント単位ではありません。

[!DNL Jackson Exporter] リストのオプションは次のとおりです。

* [マッパー機能オプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [シリアル化機能のオプション](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson] 注釈の適用

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

エクスポーターの実装は、[!DNL Sling Model] クラスのインラインで適用できる注釈をサポートすることもあり、データがどのように書き出されるのかについて、より細かいレベルで制御することが可能です。

* [[!DNL Jackson Exporter] 注釈](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## コードの表示 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## サポート資料 {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc の機能](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc の機能](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] ドキュメント](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
