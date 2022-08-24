---
title: AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定
description: アセットテンプレートを使用すると、マーケターは、デジタルアセットや印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用すると、マーケティングパンフレット、名刺、チラシ、広告、ポストカードを簡単に作成できます。 AEMでのInDesignサーバーの設定については、この節で説明します。
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 1%

---

# AEM AssetsとInDesign Serverを使用したアセットテンプレートの設定{#set-up-asset-templates-with-aem-assets-and-indesign-server}

アセットテンプレートを使用すると、マーケターは、デジタルアセットや印刷用のデジタルアセットを作成、管理および配信できます。 InDesignサーバーと統合すると、アセットテンプレートを使用すると、マーケティングパンフレット、名刺、チラシ、広告、ポストカードを簡単に作成できます。 AEMでのInDesignサーバーの設定については、この節で説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **必須** INDD テンプレートがアップロードされる際に、実行中のInDesignサーバーに接続する必要があります。 INDD ファイルでの初期処理の一部として、InDesignサーバーが必要です。

## InDesign Server体験版のダウンロード {#download-indesign-server-trial}

ダウンロード [InDesign Server体験版ダウンロード Web サイト](https://www.adobeprerelease.com/)

## 開始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
