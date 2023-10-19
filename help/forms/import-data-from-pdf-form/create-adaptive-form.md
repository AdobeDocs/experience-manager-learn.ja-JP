---
title: スキーマを作成
description: アダプティブフォームに読み込む必要があるデータに基づいて、スキーマを作成します
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 20%

---

# はじめに

最初の手順では、アダプティブフォームの入力に使用されるデータに基づいてスキーマを作成します。

## XFA はスキーマに基づいています

スキーマを使用したアダプティブフォームの作成

## XFA がスキーマに基づいていません

* XDP を AEM Forms Designer で開きます。
* ファイル／フォームのプロパティ／プレビューを選択します。。
* 「プレビューデータを生成」をクリックします。
* 「生成」をクリックします。
* 次のような意味のあるファイル名を指定します。 `form-data.xml`

無料のオンラインツールを使用して、前の手順で生成された xml データから [XSD を生成](https://www.freeformatter.com/xsd-generator.html?lang=ja)できます。

前の手順のスキーマに基づいてアダプティブフォームを作成します。

>[!NOTE]
>アダプティブフォームの送信時に生成されるデータを確認することをお勧めします。 これにより、アダプティブフォームと結合する必要のあるデータの XML 形式を適切に把握することができます。

アダプティブフォームから送信されたデータ
![submitted-data](./assets/af-submitted-data.png)

PDFから書き出されたデータ
![exported-data](./assets/exported-data.png)

書き出されたデータから、 **_topmostSubform_** ノードの名前空間が適切に保持され、データがアダプティブフォームに正常に結合されるようになります。

## 次の手順

[OSGi サービスの作成](./create-osgi-service.md)





