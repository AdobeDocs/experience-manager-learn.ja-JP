---
title: AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定
description: アセットテンプレートを使用すると、マーケティング担当者はデジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティング資料、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMでのInDesignサーバの設定については、この節で説明します。
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定{#set-up-asset-templates-with-aem-assets-and-indesign-server}

アセットテンプレートを使用すると、マーケティング担当者はデジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティング資料、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMでのInDesignサーバの設定については、この節で説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **は** 、INDDテンプレートをアップロードするときに、実行中のInDesignサーバーに接続する必要があります。 INDDファイルの初期処理の一部にはInDesignサーバーが必要です。

## InDesign Server体験版のダウンロード {#download-indesign-server-trial}

InDesign Server版の体験版ダウンロードWebサイトのダウンロード [](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## 開始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
