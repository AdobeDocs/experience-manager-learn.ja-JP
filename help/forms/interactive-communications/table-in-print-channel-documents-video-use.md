---
title: AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用
seo-title: AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用
description: 次のビデオでは、印刷チャネルのドキュメントでインタラクティブ通信で表コンポーネントを使用するために必要な手順について説明します。
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

---


# AEM Forms印刷チャネルドキュメントでの表コンポーネントの使用{#using-table-component-in-aem-forms-print-channel-document}

次のビデオでは、印刷チャネルのドキュメントでインタラクティブ通信で表コンポーネントを使用するために必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表は、データを表形式で表示する場合に使用します。 データソースから返されるデータに応じて、テーブルの行を拡大または縮小する必要があります。 印刷チャネルドキュメントでテーブルを使用するには、AEM Forms Designerを使用してレイアウトファイル（xdpファイル）を作成する必要があります。 このレイアウトファイルで、必要な列数のテーブルを追加します。 必要に応じて、列フィールドオブジェクトの種類がTextFieldまたはNumeric Fieldであることを確認します。 各列のフィールドで、データ連結が「名前による」に設定されていることを確認します。

>[!NOTE]
>
>テーブルを動的にするには、行が繰り返しとしてマークされていることを確認します。

**独自のサーバーで試す**

* [アセットファイルをダウンロードして、ハードドライブに解凍します。](assets/usingtablesinprintchannel.zip)

* パッケージマネージャーを使用して2つのzipファイルをAEMに読み込みます

* この記事に関連付けられたアセットには、次のものが含まれます。

   * レイアウトフラグメント

   * フォームデータモーダル

   * インタラクティブ通信ドキュメント
   * sampleretirementaccountdata.json

* [編集モード](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)でインタラクティブ通信ドキュメントを開きます。

* TableDemoレイアウトフラグメントを投稿セクションに追加します。
* ビデオに示すように、表のセルを適切なフォームデータモデル要素に連結します

* サンプルのjsonデータファイルを使用してインタラクティブ通信ドキュメントをプレビューします

