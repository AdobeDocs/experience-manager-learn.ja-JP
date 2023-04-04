---
title: AEM Assetsでのメタデータの読み込みと書き出しの使用
description: Adobe Experience Manager Assets のメタデータの読み込みおよび書き出し機能の使用方法について説明します。 読み込みおよび書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 2%

---

# AEM Assetsでのメタデータの読み込みと書き出しの使用 {#metadata-import-and-export}

Adobe Experience Manager Assets のメタデータの読み込みおよび書き出し機能の使用方法について説明します。 読み込みおよび書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 読み込む CSV ファイルを準備する際に、メタデータの書き出し機能を使用すると、アセットのリストを含む CSV を生成するほうが簡単です。 その後、生成された CSV ファイルを変更し、インポート機能を使用してインポートできます。

## メタデータ CSV ファイル形式 {#metadata-file-format}

### 最初の行

* CSV ファイルの最初の行で、メタデータスキーマを定義します。
* 最初の列のデフォルト値はです。 `assetPath`：アセットの絶対 JCR パスを保持します。

* 1 行目の後続の列は、アセットの他のメタデータプロパティを指します。
   * 例： `dc:title, dc:description, jcr:title`

* 単一値プロパティの形式

   * `<metadata property name> {{<property type}}`
   * プロパティタイプを指定しない場合、デフォルトでは String に設定されます。
   * 例：`dc:title {{String}}`

* プロパティ名では大文字と小文字が区別されます
   * 正しい構文： `dc:title {{String}}`
   * 誤った構文： `Dc:Title {{String}}`

* プロパティタイプでは大文字と小文字が区別されません
* すべて有効 [JCR プロパティタイプ](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) はサポートされています

* 複数値プロパティの形式 — `<metadata property name> {{<property type : MULTI }}`

### 2 行目から N 行目

* 最初の列には、アセットの絶対 JCR パスが格納されます。 例：/content/dam/asset1.jpg
* アセットのメタデータプロパティの CSV ファイルに値が含まれていない可能性があります。 その特定のアセットの欠落したメタデータプロパティは、更新されません。
