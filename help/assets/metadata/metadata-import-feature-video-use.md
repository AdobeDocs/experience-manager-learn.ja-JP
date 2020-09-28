---
title: AEM Assetsでのメタデータの読み込みと書き出しの使用
seo-title: AEM Assetsでのメタデータの読み込みと書き出しの使用
description: AEM Assetsメタデータの読み込みと書き出しの機能により、コンテンツ作成者はアセットメタデータをAEMの内外に簡単に移動でき、Microsoft Excelの機能を活用してメタデータをスケールで操作でき、AEMの既存のアセットのメタデータを一括更新できます。
seo-description: AEM Assetsメタデータの読み込みと書き出しの機能により、コンテンツ作成者はアセットメタデータをAEMの内外に簡単に移動でき、Microsoft Excelの機能を活用してメタデータをスケールで操作でき、AEMの既存のアセットのメタデータを一括更新できます。
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 3%

---


# AEM Assetsでのメタデータの読み込みと書き出しの使用{#using-metadata-import-and-export-in-aem-assets}

AEM Assetsメタデータの読み込みと書き出しの機能により、コンテンツ作成者はアセットメタデータをAEMの内外に簡単に移動でき、Microsoft Excelの機能を活用してメタデータをスケールで操作でき、AEMの既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

WebRetailスポ [ーツフォルダーのダウンロード](assets/we-retail-sports.zip)

ア [セットメタデータパッケージのダウンロード](assets/we-retail-sports-asset-metadata.zip)

## メタデータファイルの形式 {#metadata-file-format}

### CSVファイル形式

#### 最初の行

* CSVファイルの最初の行は、メタデータスキーマを定義します。
* 最初の列のデフォルトは `assetPath`に設定され、アセットの絶対JCRパスが保持されます。

* 最初の行の後続の列は、アセットの他のメタデータプロパティを指します。

   * 次に例を示します。`dc:title, dc:description, jcr:title`

* 単一値プロパティの形式

   * `<metadata property name> {{<property type}}`
   * プロパティタイプを指定しない場合、デフォルトでStringに設定されます。
   * 例：`dc:title {{String}}`

* プロパティ名では大文字と小文字が区別されます。
   * 正解`dc:title {{String}}`
   * 不正解`Dc:Ttle {{String}}`

* プロパティの種類では大文字と小文字が区別されません
* すべての有効な [JCRプロパティタイプ](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) がサポートされています

* 複数値プロパティの形式 — `<metadata property name> {{<property type : MULTI }}`

#### 2行目からN行目

* 最初の列には、アセットの絶対JCRパスが格納されます。 次に例を示します。/content/dam/asset1.jpg
* アセットのメタデータプロパティの値がCSVファイルに含まれていない可能性があります。 その特定のアセットに見つからないメタデータプロパティは更新されません。