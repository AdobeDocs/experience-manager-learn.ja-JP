---
title: Web チャネル用の最初のインタラクティブ通信の作成
seo-title: Creating your first interactive communication for the web channel
description: インタラクティブ通信は、AEM Forms 6.4 で新たに導入されました。このドキュメントでは、Web チャネル用のインタラクティブ通信を作成するために必要な手順について説明します。
seo-description: Interactive Communications is new to AEM Forms 6.4. This document will walk you through the steps needed to create an interactive communication for the web channel.
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 65b1af30-9e22-4df0-ab91-479d5406df61
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 3%

---

# Web チャネル用の最初のインタラクティブ通信の作成

インタラクティブ通信はAEM Forms 6.4 で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブ通信を作成するために必要な手順を説明します。

## 前提条件 {#prerequistes}

[パッケージマネージャーを使用して、このチュートリアルに関連するアセットをAEMにダウンロードおよび読み込みます。](assets/gettingstartedassets.zip)」を選択します。この zip ファイルには、このチュートリアルで使用する画像とドキュメントフラグメントが含まれています

[このファイルをダウンロードして展開します。](assets/warfileandswaggerfile.zip) このファイルには、Tomcat およびデータソースの設定に使用する必要のある Swagger ファイルにデプロイする必要がある SampleRest.war ファイルが含まれています。

このチュートリアルでは、次の内容について学習しました。

* データソースを作成
* フォームデータモデルの作成
* ドキュメントフラグメントを作成
* テーブルとグラフの設定
* Web チャネルドキュメントの配信
