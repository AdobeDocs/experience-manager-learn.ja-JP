---
title: AEMアダプティブFormsでのCAPTCHAの使用
seo-title: AEMアダプティブFormsでのCAPTCHAの使用
description: AEMアダプティブFormsでのCAPTCHAの追加と使用
seo-description: AEMアダプティブFormsでのCAPTCHAの追加と使用
feature: '"アダプティブForms，ワークフロー"'
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 9%

---


# AEMアダプティブFormsでのCAPTCHAの使用{#using-captchas-with-aem-adaptive-forms}

AEMアダプティブFormsでのCAPTCHAの追加と使用

[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページをご覧になって、この機能のライブデモへのリンクをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*このビデオでは、組み込みのAEM CAPTCHAサービスとGoogleのreCAPTCHAサービスの両方を使用して、AEMアダプティブフォームにCAPTCHAを追加するプロセスについて説明します。*

>[!NOTE]
>
>この機能はAEM 6.3以降でのみ使用できます。

>[!NOTE]
>
>**発行インスタンスでreCaptchaを設定するには、次の手順に従います**
>
>オーサーインスタンスでのreCaptachの設定
>
>作成者インスタンスでfelix [webコンソール](http://localhost:4502/system/console/bundles)を開きます
>
>com.adobe.granite.crypto.fileバンドルの検索
>
>バンドルIDをメモしておきます。 私の場合は20です
>
>作成者インスタンス上のファイルシステム上のバンドルIDに移動します
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMACファイルとマスター・ファイルのコピー

発行インスタンスで[felix Webコンソール](http://localhost:4502/system/console/bundles)を開きます。 com.adobe.granite.crypto.file bundleを検索します。 バンドルIDをメモしておきます。
発行インスタンスのファイルシステム上のバンドルIDに移動します。
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 既存のHMACファイルとマスターファイルを削除します。
* 作成者インスタンスからコピーしたHMACファイルとマスターファイルを貼り付けます。

AEM公開サーバーの再起動

## サポート資料{#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

