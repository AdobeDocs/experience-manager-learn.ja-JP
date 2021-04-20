---
title: AEM FormsとAdobe Campaign Standardの使い始めに
seo-title: AEM FormsとAdobe Campaign Standardの使い始めに
description: ACSキャンペーンプロファイル情報などを取得するために、AEM Formsフォームデータモデルを使用してAEM FormsをAdobe Campaign Standardと統合します。
seo-description: ACSキャンペーンプロファイル情報などを取得するために、AEM Formsフォームデータモデルを使用してAEM FormsをAdobe Campaign Standardと統合します。
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 3%

---


# AEM FormsとAdobe Campaign Standardの使い始めに{#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

このチュートリアルでは、AEM FormsとAdobe Campaign Standard(ACS)を統合する際の様々な使用例をリストします。

ACSには、豊富なAPIのセットが公開されており、ACSは当社の選択したテクノロジーとインタフェースできます。 このチュートリアルでは、ACSとのAEM Formsとのインターフェースに焦点を当てます。

AEM FormsをACSと統合するには、次の手順に従う必要があります。

* [ACSインスタンスでAPIアクセスを設定します。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON Webトークンを作成します。
* JSON WebトークンをアクセストークンのAdobeIdentity Managementサービスと交換します。
* このアクセストークンは、ACSインスタンスへのすべての要求にX-API-Keyと共に、Authorization HTTP Headerに含めます。

開始するには、次の手順に従ってください

* [このチュートリアルに関連するアセットをダウンロードして解凍します。](assets/aem-forms-and-acs-bundles.zip)
* [Felix Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイします
* Felix OSGI ConfigurationでAdobe Campaignに適した設定を指定します。
* [この記事で説明するように、サービスユーザーを作成します](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。記事に関連付けられたOSGiバンドルをデプロイしてください。
* ACS秘密鍵をetc/key/campaign/private.keyに保存します。 etc/keyの下にキャンペーンという名前のフォルダーを作成する必要があります。
* [サービスユーザー「data」にキャンペーンフォルダーへの読み取りアクセス権を付与します。](http://localhost:4502/useradmin)
