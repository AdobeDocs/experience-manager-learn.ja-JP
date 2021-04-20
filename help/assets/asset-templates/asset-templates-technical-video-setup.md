---
title: AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定
description: アセットテンプレートを使用すると、マーケティング担当者はデジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティング資料、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMでのInDesignサーバの設定については、この節で説明します。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 1%

---


# AEM AssetsとInDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}を使用したアセットテンプレートの設定

アセットテンプレートを使用すると、マーケティング担当者はデジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティング資料、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMでのInDesignサーバの設定については、この節で説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **は、INDDテンプレートをアップロードする際に、実行中のInDesignサーバーに**&#x200B;接続する必要があります。 INDDファイルの初期処理の一部にはInDesignサーバーが必要です。

## InDesign Server体験版のダウンロード{#download-indesign-server-trial}

[InDesign Server版の体験版ダウンロードWebサイト](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)をダウンロード

## InDesign Server{#starting-indesign-server}を開始しています

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
