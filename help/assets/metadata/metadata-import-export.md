---
title: AEM Assets でのメタデータの読み込みと書き出しの使用
description: Adobe Experience Manager Assets のメタデータ読み込み機能および書き出し機能の使用方法について説明します。読み込み機能と書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '378'
ht-degree: 100%

---

# AEM Assets でのメタデータの読み込みと書き出しの使用 {#metadata-import-and-export}

Adobe Experience Manager Assets のメタデータ読み込み機能および書き出し機能の使用方法について説明します。読み込み機能と書き出し機能を使用すると、コンテンツ作成者は既存のアセットのメタデータを一括更新できます。

## メタデータの書き出し {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/39396?quality=12&learn=on&captions=jpn)

>[!TIP]
>
> メタデータ書き出し CSV ファイルを Excel で開く際は、UTF-8 でエンコードされた CSV ファイルの問題を回避するように、ファイルをダブルクリックするのではなく、[Excel インポーター](https://support.microsoft.com/ja-jp/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6)を使用します。
>
> メタデータ書き出し CSV ファイルを Excel で開くには、次の手順に従います。
> 
> 1. Microsoft Excel を開きます。
> 1. __ファイル／新規__&#x200B;を選択して、空のスプレッドシートを作成します。
> 1. 空のスプレッドシートを開き、__ファイル／インポート__&#x200B;を選択します。
> 1. __テキスト__&#x200B;ファイルを選択し、「__インポート__」をクリックします
> 1. ファイルシステムから書き出された CSV ファイルを選択し、「__データの取得__」をクリックします。
> 1. インポートウィザードの手順 1 で、「__区切り__」を選択し、「__元のファイル__」を「__Unicode (UTF-8)__」に設定して、「__次へ__」をクリックします。
> 1. 手順 2 で、「__区切り文字__」を「__カンマ__」に設定し、「__次へ__」をクリックします。
> 1. 手順 3で、「__列のデータ形式__」をそのままにして、「__完了__」をクリックします。
> 1. 「__インポート__」を選択して、データをスプレッドシートに追加します

## メタデータの読み込み {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/3413185?quality=12&learn=on&captions=jpn)

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
