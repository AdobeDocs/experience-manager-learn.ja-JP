---
title: 印刷チャネルドキュメント用に2列レイアウトを作成する
seo-title: 印刷チャネルドキュメント用に2列レイアウトを作成する
description: 印刷チャネルドキュメント用に2列レイアウトを作成する
seo-description: 印刷チャネルドキュメント用に2列レイアウトを作成する
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 2%

---


# 印刷チャネルドキュメントの2列レイアウト

この短い記事では、印刷チャネルで2列のレイアウトを作成する際に必要な手順を説明します。 使用例は、ページ1の2列レイアウトとページ2の標準1列レイアウトを持つ2ページのドキュメントを生成する場合です。

以下は、AEM Formsデザイナを使用して2列レイアウトを作成する際の高度な手順です。

* 1ページ目のマスターページに2つのコンテンツ領域を作成する
* 2つのコンテンツ領域に「leftcolumn」と「rightcolumn」の名前を付けます。
* 1つのコンテンツ領域を持つ2つ目のマスターページを作成する（デフォルト）
* 「ページ編集」タブ（名称未設定サブフォーム）（ページ1）と（名称未設定サブフォーム）（ページ2）を選択し、以下のスクリーンショットに示すようにプロパティを設定します。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

ページ番号のプロパティを設定したら、「（名称未設定サブフォーム）（ページ1）」の下にサブフォームまたはターゲット領域を追加します。

次に、これらのサブフォームやターゲット領域にドキュメントフラグメントを追加します。 左の列がいっぱいになると、コンテンツは右の列にフローします。

これをローカルサーバーでテストするには、この記事に関連するアセットをダウンロードします。 このページの一番下まで下にスクロールします

* [パッケージマネージャーを使用したサンプル印刷チャネルドキュメントのダウンロードとインストール](assets/print-channel-with-two-column-layout.zip)
* [印刷チャネルドキュメントのプレビュー](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
