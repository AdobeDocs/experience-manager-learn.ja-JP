---
title: 印刷チャネルドキュメント用の 2 列レイアウトの作成
description: 印刷チャネルドキュメントの 2 列レイアウトを作成する
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '218'
ht-degree: 100%

---

# 印刷チャネルドキュメントの 2 列レイアウト

この短い記事では、印刷チャネルで 2 列のレイアウトを作成するために必要な手順を説明します。使用例は、ページ 1 が 2 列のレイアウト、ページ 2 が標準の 1 列のレイアウトを持つ 2 ページのドキュメントを生成する場合です。

AEM Forms Designer を使用して 2 列のレイアウトを作成する際の大まかな手順を次に示します。

* 1 ページ目のマスターページに 2 つのコンテンツ領域を作成する
* 2 つのコンテンツ領域に「leftcolumn」と「rightcolumn」という名前を付けます。
* 1 つのコンテンツ領域（デフォルト）を持つ 2 つ目のマスターページを作成します。
* 「ページ編集」タブ（名称未設定サブフォーム）（ページ 1）と（名称未設定サブフォーム）（ページ 2）を選択し、下のスクリーンショットに示すように、プロパティを設定します。

![ページ 1](assets/untitledsubform_paginationproperties.gif)

![ページ 2](assets/untitled_subformpage2.gif)

ページネーションプロパティを設定したら、（名称未設定サブフォーム）（ページ 1）の下にサブフォームまたはターゲット領域を追加できます。

これで、これらのサブフォームやターゲット領域にドキュメントフラグメントを追加できます。 左の列がいっぱいになると、コンテンツは右の列にフローされます。

これをローカルサーバーでテストするには、この記事に関連するアセットをダウンロードします。 このページの下部まで下にスクロールします

* [パッケージマネージャーを使用してサンプルの印刷チャネルドキュメントをダウンロードしてインストールします](assets/print-channel-with-two-column-layout.zip)
* [印刷チャネルドキュメントのプレビュー](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
