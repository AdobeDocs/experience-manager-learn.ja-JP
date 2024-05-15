---
title: AEM Assets でのメタデータの読み込みと書き出しの使用
description: Adobe Experience Manager Assets のメタデータ読み込み機能および書き出し機能の使用方法について説明します。読み込み機能と書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 100%

---

# AEM Assets でのメタデータの読み込みと書き出しの使用 {#metadata-import-and-export}

Adobe Experience Manager Assets のメタデータ読み込み機能および書き出し機能の使用方法について説明します。読み込み機能と書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 読み込む CSV ファイルを準備する際は、メタデータの書き出し機能を使用して、アセットのリストを含んだ CSV を生成する方が簡単です。その後、生成された CSV ファイルを変更し、読み込み機能を使用して読み込むことができます。

## メタデータ CSV ファイル形式 {#metadata-file-format}

### 最初の行

* CSV ファイルの最初の行で、メタデータのスキーマを定義します。
* 最初の列のデフォルト値は `assetPath` です。ここには、アセットの絶対 JCR パスを格納します。

* 最初の行の後続列は、アセットのその他のメタデータプロパティを指定します。
   * 例：`dc:title, dc:description, jcr:title`

* 単一値プロパティの形式

   * `<metadata property name> {{<property type}}`
   * プロパティタイプを指定しない場合、デフォルトでは文字列に設定されます。
   * 例：`dc:title {{String}}`

* プロパティ名では大文字と小文字が区別されます。
   * 正しい形式：`dc:title {{String}}`
   * 間違った形式：`Dc:Title {{String}}`

* プロパティタイプでは大文字と小文字が区別されません。
* すべての有効な [JCR プロパティタイプ](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)がサポートされています。

* 複数値プロパティの形式 - `<metadata property name> {{<property type : MULTI }}`

### 2 行目から N 行目

* 最初の列には、アセットの絶対 JCR パスが格納されます。 例：/content/dam/asset1.jpg
* CSV ファイルでは、アセットのメタデータプロパティに値が含まれていない場合があります。その特定のアセットの欠落したメタデータプロパティは更新されません。
