---
title: BPA および CAM プロジェクトを設定する
description: ベストプラクティスアナライザーと Cloud Acceleration Manager が、AEM as a Cloud Serviceへの移行に関するカスタマイズされたガイドを提供する方法を説明します。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 4%

---

# ベストプラクティスアナライザーと Cloud Acceleration Manager

ベストプラクティスアナライザー (BPA) と Cloud Acceleration Manager(CAM) が、AEM as a Cloud Serviceへの移行に関するカスタマイズされたガイドを提供する方法について説明します。 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## 準備状況の評価

![BPA および CAM の概要図](assets/bpa-cam-diagram.png)

BPA パッケージは、実稼動AEM 6.x 環境のクローンにインストールする必要があります。 BPA は、レポートを生成し、その後、CAM にアップロードできます。このレポートは、AEMas a Cloud Serviceに移行するために実行する必要のある主要なアクティビティに関するガイダンスを提供します。

### 重要なアクティビティ

* 実稼動 6.x 環境のクローンを作成します。 コンテンツとリファクタリングコードを移行する際に、実稼動環境のクローンを作成すると、様々なツールや変更をテストする際に役立ちます。
* 最新の BPA ツールをからダウンロードします。 [ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) およびをAEM 6.x のクローン環境にインストールします。
* BPA ツールを使用して、Cloud Acceleration Manager(CAM) にアップロードできるレポートを生成します。 CAM には、 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* CAM を使用して、AEM as a Cloud Serviceに移行するために現在のコードベースと環境に対して必要な更新に関するガイダンスを提供します。
