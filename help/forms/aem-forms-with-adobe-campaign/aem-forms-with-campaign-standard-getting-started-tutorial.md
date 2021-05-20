---
title: AEM FormsとAdobe Campaign Standardの概要
seo-title: AEM FormsとAdobe Campaign Standardの概要
description: AEM Formsフォームデータモデルを使用してAEM FormsとAdobe Campaign Standardを統合し、ACSキャンペーンのプロファイル情報などを取得する
seo-description: AEM Formsフォームデータモデルを使用してAEM FormsとAdobe Campaign Standardを統合し、ACSキャンペーンのプロファイル情報などを取得する
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 3%

---


# AEM FormsとAdobe Campaign Standardの概要{#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

このチュートリアルでは、AEM FormsとAdobe Campaign Standard(ACS)の統合に関する様々な使用例を示します。

ACSは、APIの豊富なセットを公開し、ACSを当社が選択したテクノロジーとインターフェイスできます。 このチュートリアルでは、AEM FormsとACSとのやり取りに専念します。

AEM FormsをACSと統合するには、次の手順に従う必要があります。

* [ACSインスタンスでAPIアクセスを設定します。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON Webトークンを作成します。
* JSON Webトークンをアクセストークン用にAdobeIdentity Managementサービスと交換します。
* このアクセストークンを、ACSインスタンスへのすべてのリクエストでX-API-Keyと共に、認証HTTPヘッダーに含めます。

利用を開始するには、次の手順に従ってください

* [このチュートリアルに関連するアセットをダウンロードして解凍します。](assets/aem-forms-and-acs-bundles.zip)
* [Felix Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイします。
* Felix OSGI設定でAdobe Campaignに適切な設定を指定します。
* [この記事で説明しているサービスユーザーを作成します](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。記事に関連付けられたOSGiバンドルをデプロイしてください。
* ACS秘密鍵をetc/key/campaign/private.keyに保存します。 etc/keyの下にcampaignというフォルダーを作成する必要があります。
* [サービスユーザー「data」にキャンペーンフォルダーへの読み取りアクセス権を付与します。](http://localhost:4502/useradmin)
