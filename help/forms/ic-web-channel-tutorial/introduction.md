---
title: Webチャネルー向けの最初のインタラクティブな通信の作成
seo-title: Webチャネルー向けの最初のインタラクティブな通信の作成
description: Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、Webチャネル用の対話型コミュニケーションを作成するために必要な手順を順を追って説明します。
seo-description: Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、Webチャネル用の対話型コミュニケーションを作成するために必要な手順を順を追って説明します。
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 2%

---


# Webチャネルー向けの最初のインタラクティブな通信の作成

Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブな通信を作成するために必要な手順を順を追って説明します。

この機能のライブデモへのリンクは、 [AEM Formsのサンプルページをご覧ください](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 。

## 前提条件 {#prerequistes}

[パッケージマネージャーを使用して、このチュートリアルに関連するアセットをAEMにダウンロードし、読み込みます。](assets/gettingstartedassets.zip)」を選択します。このzipファイルには、このチュートリアルで使用する画像とドキュメントフラグメントが含まれています

[このファイルをダウンロードして解凍します。](assets/warfileandswaggerfile.zip) このファイルには、Tomcatおよびデータソースの設定に使用する必要のあるSwaggerファイルにデプロイする必要があるSampleRest.warファイルが含まれています。

このチュートリアルを完了する際には、次の内容を学習しています。

* データソースの作成
* フォームデータモデルを作成
* ドキュメントフラグメントの作成
* テーブルとグラフの設定
* Webチャネルドキュメントの配信




