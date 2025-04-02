---
title: AEM Forms 印刷チャネルドキュメントでのテーブルコンポーネントの使用
description: 次のビデオでは、印刷チャネルドキュメントに対してインタラクティブ通信でテーブルコンポーネントを使用するために必要な手順を説明します。
feature: Interactive Communication
doc-type: technical video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 277
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '258'
ht-degree: 100%

---

# AEM Forms 印刷チャネルドキュメントでのテーブルコンポーネントの使用 {#using-table-component-in-aem-forms-print-channel-document}

次のビデオでは、印刷チャネルドキュメントに対してインタラクティブ通信でテーブルコンポーネントを使用するために必要な手順を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

テーブルは、データを表形式で表示するために使用されます。 テーブルの行は、データソースから返されるデータに応じて拡大または縮小する必要があります。 印刷チャネルドキュメントでテーブルを使用するには、AEM Forms Designer を使用してレイアウトファイル（xdp ファイル）を作成する必要があります。 このレイアウトファイルで、必要な列数のテーブルを追加します。 必要に応じて、列フィールドオブジェクトのタイプが「テキストフィールド」または「数値フィールド」であることを確認します。 各列のフィールドで、データ連結が「名前による」に設定されていることを確認します。

>[!NOTE]
>
>テーブルを動的にするには、行を繰り返しとしてマークしていることを確認します。

**独自のサーバーで試す**

* [アセットファイルをダウンロードし、ハードドライブに解凍します](assets/usingtablesinprintchannel.zip)

* パッケージマネージャーを使用して 2 つの zip ファイルを AEM に読み込みます

* この記事に関連付けられたアセットには、次のものが含まれます。

   * レイアウトフラグメント

   * フォームデータモーダル

   * インタラクティブ通信ドキュメント
   * sampleretirementaccountdata.json

* [編集モード](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)でインタラクティブ通信ドキュメントを開きます。

* 投稿セクションに TableDemo レイアウトフラグメントを追加します。
* ビデオに示すように、テーブルのセルを適切なフォームデータモデル要素に連結します

* サンプルの JSON データファイルを使用して、インタラクティブ通信ドキュメントをプレビューします
