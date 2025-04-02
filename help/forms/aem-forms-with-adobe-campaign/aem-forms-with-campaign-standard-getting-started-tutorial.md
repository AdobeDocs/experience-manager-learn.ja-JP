---
title: AEM Forms と Adobe Campaign Standard の統合
description: AEM Forms フォームデータモデルを使用して AEM Forms と Adobe Campaign Standard を統合し、ACS キャンペーンプロファイル情報などを取得します。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '238'
ht-degree: 100%

---

# AEM Forms と Adobe Campaign Standard の統合

![formsandcampaign](assets/helpx-cards-forms.png)

AEM Forms と Adobe Campaign Standard（ACS）の統合方法を説明します。

ACS には豊富な API のセットが公開されており、ACS を好みの技術とインターフェイスさせることができます。このチュートリアルでは、AEM Forms と ACS とのインターフェイスに専念します。

AEM Forms を ACS と統合するには、次の手順に従う必要があります。

* [ACS インスタンスで API アクセスを設定します。](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=ja)
* JSON web トークンを作成します。
* アクセストークンの JSON web トークンを Adobe Identity Management サービスと交換します。
* このアクセストークンを認証 HTTP ヘッダーに含め、ACS インスタンスへのすべてのリクエストで X-API-Key と共に含めます。

利用を開始するには、次の手順に従ってください

* [このチュートリアルに関連するアセットをダウンロードして展開します。](assets/aem-forms-and-acs-bundles.zip)
* [Felix web コンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイします
* Felix OSGi 設定で、Adobe Campaign に適切な設定を指定します。
* [この記事で説明されているサービスユーザーを作成します](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。記事に関連付けられた OSGi バンドルをデプロイします。
* ACS 秘密鍵を etc/key/campaign/private.key に保存します。 etc/key の下に campaign というフォルダーを作成する必要があります。
* [サービスユーザー「data」にキャンペーンフォルダーへの読み取りアクセス権を付与します。](http://localhost:4502/useradmin)

## 次の手順

[JWT とアクセストークンの生成](partone.md)
