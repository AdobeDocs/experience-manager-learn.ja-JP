---
title: AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用
seo-title: AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用
description: 次のビデオでは、印刷チャネルドキュメントに対してInteractive Communicationsの表コンポーネントを使用するために必要な手順について説明します。
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用{#using-table-component-in-aem-forms-print-channel-document}

次のビデオでは、印刷チャネルドキュメントに対してInteractive Communicationsの表コンポーネントを使用するために必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

テーブルは、データを表形式で表示するために使用します。 テーブルの行は、データソースから返されるデータに応じて拡大または縮小する必要があります。 印刷チャネルドキュメントでテーブルを使用するには、AEM Formsデザイナを使用してレイアウトファイル（xdpファイル）を作成する必要があります。 このレイアウトファイルで、必要な列数のテーブルを追加します。 必要に応じて、列フィールドオブジェクトのタイプがTextFieldまたはNumeric Fieldであることを確認します。 各列について、フィールドのデータ連結が「名前を使用」に設定されていることを確認します。

>[!NOTE]
>
>テーブルを動的にするには、行を繰り返しとしてマークしていることを確認します。

**独自のサーバーで試してみます**

* [アセットファイルをハードドライブにダウンロードして解凍します。](assets/usingtablesinprintchannel.zip)

* パッケージマネージャーを使用して、2つのzipファイルをAEMに読み込みます

* この記事に関連付けられたアセットには、次のものが含まれます。

   * レイアウトフラグメント

   * Form Data Modal

   * 対話型通信ドキュメント
   * sampleretirementaccountdata.json

* [編集モード](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)で対話型通信ドキュメントを開きます。

* 貢献追加度セクションへのTableDemoレイアウトフラグメント。
* ビデオで示すように、表のセルを適切なフォームデータモデルの要素に連結します

* 提供されたサンプルjsonデータファイルを含むプレビューインタラクティブ通信ドキュメント

