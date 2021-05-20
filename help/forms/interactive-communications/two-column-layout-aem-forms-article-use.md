---
title: 印刷チャネルドキュメント用の2列レイアウトの作成
seo-title: 印刷チャネルドキュメント用の2列レイアウトの作成
description: 印刷チャネルドキュメント用の2列レイアウトの作成
seo-description: 印刷チャネルドキュメント用の2列レイアウトの作成
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 2%

---


# 印刷チャネルドキュメントの2列レイアウト

この短い記事では、印刷チャネルで2列のレイアウトを作成するために必要な手順を説明します。 使用例は、ページ1に2列のレイアウト、ページ2に標準の1列のレイアウトを含む2ページのドキュメントを生成する場合です。

AEM Forms Designerを使用して2列レイアウトを作成する際の大まかな手順を次に示します。

* 1ページ目のマスターページに2つのコンテンツ領域を作成する
* 2つのコンテンツ領域に「leftcolumn」と「rightcolumn」という名前を付けます。
* 1つのコンテンツ領域（デフォルト）を持つ2つ目のマスターページを作成します。
* 「ページ編集」タブ（名称未設定サブフォーム）（ページ1）および（名称未設定サブフォーム）（ページ2）を選択し、以下のスクリーンショットに示すように、プロパティを設定します。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

ページネーションプロパティを設定したら、（名称未設定サブフォーム）（ページ1）の下にサブフォームまたはターゲット領域を追加できます。

次に、これらのサブフォームやターゲット領域にドキュメントフラグメントを追加します。 左側の列がいっぱいになると、コンテンツは右側の列にフローされます。

これをローカルサーバーでテストするには、この記事に関連するアセットをダウンロードします。 このページの下部まで下にスクロールします

* [パッケージマネージャーを使用してサンプル印刷チャネルドキュメントをダウンロードしてインストールする](assets/print-channel-with-two-column-layout.zip)
* [印刷チャネルドキュメントのプレビュー](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
