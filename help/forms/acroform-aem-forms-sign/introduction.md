---
title: AEM Formsを使用した Acroforms
description: Acroform を使用したアダプティブフォームの作成と、データの結合を使用したPDF。 結合されたデータを含むPDFは、Acrobat Signを使用した署名用に送信できます。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 4%

---


# Acroforms からのアダプティブFormsの作成

組織は様々な形式を持っています。 これらのフォームの一部はMicrosoft Word で作成され、PDFに変換されます。 デフォルトでは、これらのフォームはAdobe ReaderまたはAcrobatを使用して入力できません。 AcrobatまたはReaderを使用してこれらのフォームを入力できるようにするには、これらのフォームを Acroform に変換する必要があります。 Acroforms は、Acrobatを使用して作成されたフォームです。 この記事では、Acroform からアダプティブフォームを作成し、データを Acroform に結合してPDFを取得する方法について説明します。 結合されたデータを含むPDFは、Acrobat Signを使用した署名用に送信することもできます。

>[!NOTE]
>
>AEM Forms 6.5 を使用している場合は、Automated forms conversion機能を使用してください。

## 前提条件

* AEM Forms 6.3 または 6.4 がインストールおよび設定済み
* Adobe Acrobatへのアクセス
* AEM/AEM Formsに精通。

### この機能をシステムで動作させるには、次が必要です

* を使用してバンドルをダウンロードおよびデプロイします [Felix Web コンソール](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [このパッケージをダウンロードしてAEMにインポート](assets/acro-form-aem-form.zip). このパッケージには、acroform から XSD を作成するサンプルワークフローと HTML ページが含まれています。
* を開きます。 [configMgr](http://localhost:4502/system/console/configMgr)
   * 「Apache Sling Service User Mapper Service」を検索し、クリックしてプロパティを開きます。
   * 次をクリック： `+` アイコン（プラス）をクリックして、次のサービスマッピングを追加します。
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 「保存」をクリックします。
