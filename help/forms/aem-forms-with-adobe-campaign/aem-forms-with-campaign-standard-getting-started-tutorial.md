---
title: AEM FormsとAdobe Campaign Standardの概要
description: AEM Formsフォームデータモデルを使用してAEM FormsとAdobe Campaign Standardを統合し、ACS キャンペーンプロファイル情報などを取得します。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 3%

---

# AEM FormsとAdobe Campaign Standardの概要 {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

このチュートリアルでは、AEM FormsとAdobe Campaign Standard(ACS) の統合に関する様々な使用例を示します。

ACS は、API の豊富なセットが公開され、ACS を選択したテクノロジーと連携させることができます。 このチュートリアルでは、AEM Formsと ACS とのインターフェースに専念します。

AEM Formsを ACS と統合するには、次の手順に従う必要があります。

* [ACS インスタンスで API アクセスを設定します。](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* JSON Web トークンを作成します。
* アクセストークンの JSON Web トークンをAdobeIdentity Management Service と交換します。
* このアクセストークンを認証 HTTP ヘッダーに含め、ACS インスタンスへのすべてのリクエストで X-API-Key と共に含めます。

利用を開始するには、次の手順に従ってください

* [このチュートリアルに関連するアセットをダウンロードして展開します。](assets/aem-forms-and-acs-bundles.zip)
* 次を使用してバンドルをデプロイ [Felix Web コンソール](http://localhost:4502/system/console/bundles)
* Felix OSGi 設定で、Adobe Campaignに適切な設定を指定します。
* [この記事で説明されているサービスユーザーの作成](/help/forms/adaptive-forms/service-user-tutorial-develop.md). 記事に関連付けられた OSGi バンドルをデプロイします。
* ACS 秘密鍵をetc/key/campaign/private.keyに保存します。 etc/key の下に campaign というフォルダーを作成する必要があります。
* [サービスユーザー「data」にキャンペーンフォルダーへの読み取りアクセス権を付与します。](http://localhost:4502/useradmin)
