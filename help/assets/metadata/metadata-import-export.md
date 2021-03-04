---
title: AEM Assetsでのメタデータの読み込みと書き出しの使用
description: Adobe Experience Managerアセットのメタデータの読み込み機能と書き出し機能の使用方法を説明します。 読み込みと書き出しの機能により、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。
version: 6.3, 6.4, 6.5, cloud-service
topic: コンテンツ管理
feature: メタデータ
role: 管理者
level: 中間
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# AEM Assetsでのメタデータの読み込みと書き出しの使用{#metadata-import-and-export}

Adobe Experience Managerアセットのメタデータの読み込み機能と書き出し機能の使用方法を説明します。 読み込みと書き出しの機能により、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 読み込むCSVファイルを準備する場合、メタデータ書き出し機能を使用すると、アセットのリストを含むCSVを簡単に生成できます。 その後、生成したCSVファイルを変更し、「読み込み」機能を使用して読み込むことができます。

## メタデータCSVファイル形式{#metadata-file-format}

### 最初の行

* CSVファイルの最初の行は、メタデータスキーマを定義します。
* 最初の列はデフォルトで`assetPath`に設定され、アセットの絶対JCRパスが保持されます。

* 最初の行の後続の列は、アセットの他のメタデータプロパティを指します。
   * 例：`dc:title, dc:description, jcr:title`

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

### 2行目からN行目

* 最初の列には、アセットの絶対JCRパスが格納されます。 次に例を示します。/content/dam/asset1.jpg
* アセットのメタデータプロパティの値がCSVファイルに含まれていない可能性があります。 その特定のアセットに見つからないメタデータプロパティは更新されません。
