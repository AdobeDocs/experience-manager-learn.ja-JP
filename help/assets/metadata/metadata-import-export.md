---
title: AEM Assetsでのメタデータの読み込みと書き出しの使用
description: Adobe Experience Manager Assetsのメタデータの読み込みおよび書き出し機能の使用方法について説明します。 読み込みおよび書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。
version: 6.3, 6.4, 6.5, cloud-service
topic: コンテンツ管理
feature: メタデータ
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 4%

---


# AEM Assetsでのメタデータの読み込みと書き出しの使用{#metadata-import-and-export}

Adobe Experience Manager Assetsのメタデータの読み込みおよび書き出し機能の使用方法について説明します。 読み込みおよび書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 読み込むCSVファイルを準備する際に、メタデータの書き出し機能を使用すると、アセットのリストを含むCSVを生成するほうが簡単です。 その後、生成されたCSVファイルを変更し、インポート機能を使用してインポートできます。

## メタデータCSVファイル形式{#metadata-file-format}

### 最初の行

* CSVファイルの最初の行で、メタデータスキーマが定義されます。
* 最初の列のデフォルト値は`assetPath`で、アセットのJCRパスの絶対パスが格納されます。

* 1行目の後続の列は、アセットの他のメタデータプロパティを指します。
   * 次に例を示します。`dc:title, dc:description, jcr:title`

* 単一値プロパティの形式

   * `<metadata property name> {{<property type}}`
   * プロパティタイプを指定しない場合、デフォルトはStringになります。
   * 例：`dc:title {{String}}`

* プロパティ名では大文字と小文字が区別されます
   * 正解`dc:title {{String}}`
   * 不正解`Dc:Title {{String}}`

* プロパティタイプでは大文字と小文字が区別されません
* すべての有効な[JCRプロパティタイプ](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)がサポートされています

* 複数値プロパティの形式 — `<metadata property name> {{<property type : MULTI }}`

### 2行目からN行目

* 最初の列には、アセットの絶対JCRパスが格納されます。 例：/content/dam/asset1.jpg
* アセットのメタデータプロパティのCSVファイルに値が含まれていない可能性があります。 その特定のアセットの見つからないメタデータプロパティは、更新されません。
