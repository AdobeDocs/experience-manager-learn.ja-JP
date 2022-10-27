---
title: AEM Adaptive Formsでの CAPTCHA の使用
description: AEMアダプティブFormsでの CAPTCHA の追加と使用
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 11%

---

# AEM Adaptive Formsでの CAPTCHA の使用{#using-captchas-with-aem-adaptive-forms}

AEMアダプティブFormsでの CAPTCHA の追加と使用

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*このビデオでは、組み込みのAEM CAPTCHA サービスとGoogleの reCAPTCHA サービスの両方を使用して、AEMアダプティブフォームに CAPTCHA を追加するプロセスを順を追って説明します。*

>[!NOTE]
>
>この機能は、AEM 6.3 以降でのみ使用できます。

>[!NOTE]
>
>**パブリッシュインスタンスで reCaptcha を設定するには、次の手順に従います**
>
>オーサーインスタンスでの reCaptach の設定
>
>フェリックスを開く [web コンソール](http://localhost:4502/system/console/bundles) オーサーインスタンス上
>
>com.adobe.granite.crypto.file バンドルを検索します。
>
>バンドル ID をメモしておきます。 インスタンスでは 20 です
>
>オーサーインスタンス上のファイルシステムのバンドル ID に移動します。
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMAC ファイルとマスターファイルをコピーする
>
を開きます。 [felix web コンソール](http://localhost:4502/system/console/bundles) をパブリッシュインスタンス上に置きます。 com.adobe.granite.crypto.file bundle を検索します。 バンドル ID をメモします。
パブリッシュインスタンスのファイルシステム上のバンドル ID に移動します。
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 既存の HMAC ファイルとマスターファイルを削除します。
* オーサーインスタンスからコピーした HMAC ファイルとマスターファイルを貼り付けます。
>
AEM Publish Server を再起動します。

## サポート資料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
