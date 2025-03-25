---
title: AEM Adaptive Forms での CAPTCHA の使用
description: AEM Adaptive Forms での CAPTCHA の追加と使用
feature: Adaptive Forms,Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 100%

---

# AEM Adaptive Forms での CAPTCHA の使用{#using-captchas-with-aem-adaptive-forms}

AEM Adaptive Forms での CAPTCHA の追加と使用

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*このビデオでは、組み込みの AEM CAPTCHA サービスと Google の reCAPTCHA サービスの両方を使用して、AEM Adaptive Forms に CAPTCHA を追加するプロセスを順を追って説明します。*

>[!NOTE]
>
>この機能は、AEM 6.3 以降でのみ使用できます。

>[!NOTE]
>
>**公開インスタンスで reCAPTCHA を設定するには、次の手順に従います**
>
>オーサーインスタンスでの reCAPTCHA の設定
>
>オーサーインスタンスで Felix [web コンソール](http://localhost:4502/system/console/bundles)を開きます。
>
>com.adobe.granite.crypto.file バンドルを検索します。
>
>バンドル ID をメモしておきます。 インスタンスでは 20 です
>
>オーサーインスタンス上のファイルシステムのバンドル ID に移動します。
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMAC ファイルおよびマスターファイルをコピーします。
>
パブリッシュインスタンスで [Felix web コンソール](http://localhost:4502/system/console/bundles)を開きます。 com.adobe.granite.crypto.file バンドルを検索します。 バンドル ID をメモします。
>
パブリッシュインスタンスのファイルシステム上のバンドル ID に移動します。
>
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 既存の HMAC ファイルとマスターファイルを削除します。
* オーサーインスタンスからコピーした HMAC ファイルとマスターファイルを貼り付けます。
>
AEM Publish サーバーを再起動します。

## サポート資料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
