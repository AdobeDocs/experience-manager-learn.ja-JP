---
title: AEM Adaptive FormsでのCAPTCHAの使用
description: AEM Adaptive FormsでのCAPTCHAの追加と使用
feature: アダプティブForms，ワークフロー
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 10%

---


# AEM Adaptive FormsでのCAPTCHAの使用{#using-captchas-with-aem-adaptive-forms}

AEM Adaptive FormsでのCAPTCHAの追加と使用

この機能のライブデモへのリンクについては、[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1)ページを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*このビデオでは、組み込みのAEM CAPTCHAサービスとGoogleのreCAPTCHAサービスの両方を使用して、AEMアダプティブフォームにCAPTCHAを追加するプロセスについて説明します。*

>[!NOTE]
>
>この機能は、AEM 6.3以降でのみ使用できます。

>[!NOTE]
>
>**パブリッシュインスタンスでreCaptchaを設定するには、次の手順に従います**
>
>オーサーインスタンスでのreCaptachの設定
>
>オーサーインスタンスでFelix [webコンソール](http://localhost:4502/system/console/bundles)を開きます。
>
>com.adobe.granite.crypto.fileバンドルを検索します。
>
>バンドルIDをメモしておきます。 インスタンスでは、20
>
>オーサーインスタンス上のファイルシステムのバンドルIDに移動します。
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMACファイルとマスターファイルのコピー

パブリッシュインスタンスで[felix Webコンソール](http://localhost:4502/system/console/bundles)を開きます。 com.adobe.granite.crypto.fileバンドルを検索します。 バンドルIDをメモします。
パブリッシュインスタンスのファイルシステム上のバンドルIDに移動します。
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 既存のHMACファイルとマスターファイルを削除します。
* オーサーインスタンスからコピーしたHMACファイルとマスターファイルを貼り付けます。

AEMパブリッシュサーバーを再起動します。

## サポート資料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

