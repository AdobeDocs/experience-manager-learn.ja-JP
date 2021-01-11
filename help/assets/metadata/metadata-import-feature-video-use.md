---
title: AEM Assetsでのメタデータの読み込みと書き出しの使用
description: AEM Assetsメタデータの読み込みと書き出しの機能により、コンテンツ作成者はアセットメタデータをAEMの内外に簡単に移動でき、Microsoft Excelの機能を活用してメタデータをスケールで操作でき、AEMの既存のアセットのメタデータを一括更新できます。
topics: metadata
audience: all
doc-type: feature video
activity: use
kt: 647
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 2c0818d0223a3db55e6407068f4802b9e7f7dd83
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---


# AEM Assetsでのメタデータの読み込みと書き出しの使用{#using-metadata-import-and-export-in-aem-assets}

AEM Assetsメタデータの読み込みと書き出しの機能により、コンテンツ作成者はアセットメタデータをAEMの内外に簡単に移動でき、Microsoft Excelの機能を活用してメタデータをスケールで操作でき、AEMの既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

[WeRetailスポーツフォルダー](assets/we-retail-sports.zip)をダウンロード

[アセットメタデータパッケージ](assets/we-retail-sports-asset-metadata.zip)をダウンロード

## メタデータファイル形式{#metadata-file-format}

### CSVファイル形式

#### 最初の行

* CSVファイルの最初の行は、メタデータスキーマを定義します。
* 最初の列はデフォルトで`assetPath`に設定され、アセットの絶対JCRパスが保持されます。

* 最初の行の後続の列は、アセットの他のメタデータプロパティを指します。

   * 例： : `dc:title, dc:description, jcr:title`

* 単一値プロパティの形式

   * `<metadata property name> {{<property type}}`
   * プロパティタイプを指定しない場合、デフォルトでStringに設定されます。
   * 例：`dc:title {{String}}`

* プロパティ名では大文字と小文字が区別されます。
   * 正解`dc:title {{String}}`
   * 不正解`Dc:Title {{String}}`

* プロパティの種類では大文字と小文字が区別されません
* すべての有効な[JCRプロパティタイプ](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)がサポートされます

* 複数値プロパティの形式 — `<metadata property name> {{<property type : MULTI }}`

#### 2行目からN行目

* 最初の列には、アセットの絶対JCRパスが格納されます。 次に例を示します。/content/dam/asset1.jpg
* アセットのメタデータプロパティの値がCSVファイルに含まれていない可能性があります。 その特定のアセットに見つからないメタデータプロパティは更新されません。
