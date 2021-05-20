---
title: Webチャネル用の最初のインタラクティブ通信の作成
seo-title: Webチャネル用の最初のインタラクティブ通信の作成
description: AEM Forms 6.4では、インタラクティブ通信が新たに導入されました。このドキュメントでは、Webチャネル用のインタラクティブ通信を作成するために必要な手順について説明します。
seo-description: AEM Forms 6.4では、インタラクティブ通信が新たに導入されました。このドキュメントでは、Webチャネル用のインタラクティブ通信を作成するために必要な手順について説明します。
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

---


# Webチャネル用の最初のインタラクティブ通信の作成

AEM Forms 6.4では、Interactive Communicationsが新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブ通信を作成するために必要な手順について説明します。

この機能のライブデモへのリンクについては、[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページを参照してください。

## 前提条件 {#prerequistes}

[パッケージマネージャーを使用して、このチュートリアルに関連するアセットをAEMにダウンロードおよび読み込みます。](assets/gettingstartedassets.zip)」を選択します。このzipファイルには、このチュートリアルで使用する画像とドキュメントフラグメントが含まれています

[このファイルをダウンロードして解凍します。](assets/warfileandswaggerfile.zip) このファイルには、Tomcatにデプロイする必要があるSampleRest.warファイルと、データソースの設定に使用する必要があるSwaggerファイルが含まれています。

このチュートリアルでは、次の内容について学習します。

* データソースの作成
* フォームデータモデルを作成
* ドキュメントフラグメントの作成
* テーブルとグラフの設定
* Webチャネルドキュメントの配信




