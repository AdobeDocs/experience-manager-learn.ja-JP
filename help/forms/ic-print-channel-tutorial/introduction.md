---
title: 印刷チャネル用の最初のインタラクティブ通信の作成
seo-title: Creating your first interactive communication for the print channel
description: インタラクティブ通信はAEM Forms 6.4 で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブ通信を作成するために必要な手順を説明します。
seo-description: Interactive Communications is new to AEM Forms 6.4. This document will walk you through the steps needed to create an interactive communication for the print channel.
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1949aeff-ae56-4abd-8e63-23c2fb4859f2
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 印刷チャネル用の最初のインタラクティブ通信の作成

インタラクティブ通信はAEM Forms 6.4 で新たに導入されました。このドキュメントでは、印刷チャネル用のインタラクティブ通信を作成するために必要な手順を説明します。

## 前提条件 {#prerequistes}

[パッケージマネージャーを使用して、このチュートリアルに関連するアセットをAEMにダウンロードおよび読み込みます。](assets/gettingstartedassets.zip)この zip ファイルには、アセットパッケージの一部として、画像、ドキュメントフラグメント、監視フォルダー設定およびレイアウトファイル (xdp) が含まれています

[このファイルをダウンロードして展開します。](assets/warfileandswaggerfile.zip) このファイルには、Tomcat およびデータソースの設定に使用する必要のある Swagger ファイルにデプロイする必要がある SampleRest.war ファイルが含まれています。

このチュートリアルでは、次の内容について学習しました。

* データソースを作成
* フォームデータモデルの作成
* ドキュメントフラグメントを作成
* テーブルとグラフの設定
* 監視フォルダーを使用してバッチモードでドキュメントを生成する
