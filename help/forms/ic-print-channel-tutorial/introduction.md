---
title: 印刷チャネル向けの最初の対話型通信を作成する
seo-title: 印刷チャネル向けの最初の対話型通信を作成する
description: Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブな通信を作成するために必要な手順を順を追って説明します。
seo-description: Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブな通信を作成するために必要な手順を順を追って説明します。
feature: 対話型通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 2%

---


# 印刷チャネル向けの最初の対話型通信を作成する

Interactive Communicationsは、AEM Forms6.4で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブな通信を作成するために必要な手順を順を追って説明します。

[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページをご覧になって、この機能のライブデモへのリンクをご覧ください。

## 前提条件 {#prerequistes}

[パッケージマネージャーを使用して、このチュートリアルに関連するアセットをAEMにダウンロードし、読み込みます。](assets/gettingstartedassets.zip)このzipファイルには、画像、ドキュメントフラグメント、監視フォルダー設定およびレイアウトファイル(xdp)がアセットパッケージの一部として含まれています

[このファイルをダウンロードして解凍します。](assets/warfileandswaggerfile.zip) このファイルには、Tomcatおよびデータソースの設定に使用する必要のあるSwaggerファイルにデプロイする必要があるSampleRest.warファイルが含まれています。

このチュートリアルを完了する際には、次の内容を学習しています。

* データソースの作成
* フォームデータモデルを作成
* ドキュメントフラグメントの作成
* テーブルとグラフの設定
* 監視フォルダーを使用したバッチモードでのドキュメントの生成

