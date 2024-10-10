---
title: AEM Assets および InDesign Server を使用したアセットテンプレートの設定
description: アセットテンプレートを使用すると、マーケターは、デジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。マーケティングパンフレット、名刺、チラシ、広告、ポストカードなどの作成は、InDesign Server と統合されたアセットテンプレートでより簡単に行うことができます。AEM との InDesign Server の設定については、この節で説明します。
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '153'
ht-degree: 100%

---

# AEM Assets および InDesign Server を使用したアセットテンプレートの設定{#set-up-asset-templates-with-aem-assets-and-indesign-server}

アセットテンプレートを使用すると、マーケターは、デジタルおよび印刷用のデジタルアセットを作成、管理および配信できます。マーケティングパンフレット、名刺、チラシ、広告、ポストカードなどの作成は、InDesign Server と統合されたアセットテンプレートでより簡単に行うことができます。AEM との InDesign Server の設定については、この節で説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM は、INDD テンプレートがアップロードされる際に、実行中の InDesign Server に接続されている&#x200B;**必要があります**。 INDD ファイルでの初期処理の一部として、InDesign Server が必要です。

## InDesign Server 体験版のダウンロード {#download-indesign-server-trial}

[InDesign Server 体験版ダウンロード web サイト](https://www.adobeprerelease.com/)でダウンロード

## InDesign Server の起動 {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
