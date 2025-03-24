---
title: Acroforms と AEM Forms の連携
description: Acroform を使用したアダプティブ フォームの作成と、データをマージして PDF の取得に関して、順を追って説明するチュートリアル。データを結合した PDF は、Acrobat Sign を使用した署名用に送信することができます。
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 100%

---


# Acroforms からのアダプティブフォームの作成

組織には様々なフォームがあります。 これらのフォームの一部は Microsoft Word で作成され、PDF に変換されます。 これらのフォームはデフォルトでは、Adobe Reader または Acrobat を使用して入力することができません。 これらのフォームに Acrobat または Reader を使用して入力できるようにするには、これらのフォームを Acroform に変換する必要があります。Acroforms は、Acrobat を使用して作成されたフォームです。 この記事では、Acroform からアダプティブフォームを作成し、データを Acroform にマージして戻し、PDF を取得する方法について順を追って説明します。結合されたデータを含む PDF は、Acrobat Sign を使用した署名用に送信することもできます。

>[!NOTE]
>
>AEM Forms 6.5 をご使用の場合は、自動フォーム変換機能を使用してください。

## 前提条件

* AEM Forms 6.3 または 6.4 がインストールおよび設定済み
* Adobe Acrobat へのアクセス
* AEM や AEM Forms に精通していること。

### この機能をシステムで動作させるための必要項目

* [Felix web コンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをダウンロードおよびデプロイ
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [このパッケージをダウンロードして AEM に読み込む](assets/acro-form-aem-form.zip) このパッケージには、Acroform から XSD を作成するサンプルワークフローと HTML ページが含まれています。
* [configMgr](http://localhost:4502/system/console/configMgr) を開きます。
   * 「Apache Sling Service User Mapper Service」を検索し、クリックしてプロパティを開く
   * `+` アイコン（プラス）をクリックして、次のサービスマッピングを追加します。
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 「保存」をクリック
