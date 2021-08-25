---
title: AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定
description: アセットテンプレートを使用すると、マーケターは、デジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティングカタログ、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMを使用したInDesignサーバーの設定については、この節で説明します。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定{#set-up-asset-templates-with-aem-assets-and-indesign-server}

アセットテンプレートを使用すると、マーケターは、デジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用してマーケティングカタログ、名刺、チラシ、広告、およびはがきを簡単に作成できます。 AEMを使用したInDesignサーバーの設定については、この節で説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **は、INDDテンプレートをアップロードする際に、実行中のInDesignサーバーに接続する必要があります。** INDDファイルでの初期処理の一部には、InDesignサーバーが必要です。

## 体験版InDesign Serverのダウンロード {#download-indesign-server-trial}

[InDesign Server体験版ダウンロードWebサイト](https://www.adobe.com/devnet/premiere/sdk/cs5/indesign-server-trial-downloads.html)をダウンロード

## 開始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
