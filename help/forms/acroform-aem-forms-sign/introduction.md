---
title: AEM Formsを使用したAcroforms
description: Acroformを使用したアダプティブフォームの作成と、データの結合によるPDFの取得に関する手順を示すチュートリアルです。 その後、データがマージされたPDFを、Adobe Signを使用した署名用に送信できます。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 4%

---


# AcroformsからのアダプティブFormsの作成

組織は様々なフォームを持っています。 これらのフォームの一部はMicrosoft Wordで作成され、PDFに変換されます。 デフォルトでは、これらのフォームはAdobe ReaderまたはAcrobatを使用して入力できません。 AcrobatまたはReaderを使用してこれらのフォームを入力できるようにするには、これらのフォームをAcroformに変換する必要があります。 Acroformsは、Acrobatを使用して作成されたフォームです。 この記事では、Acroformからアダプティブフォームを作成し、データをAcroformにマージしてPDFを取得する手順について説明します。 データがマージされたPDFも、Adobe Signを使用した署名用に送信できます。

>[!NOTE]
>
>AEM Forms 6.5を使用している場合は、Automated forms conversion機能を使用してください。

## 前提条件

* AEM Forms 6.3または6.4がインストールおよび設定済み
* Adobe Acrobatへのアクセス
* AEM/AEM Formsに精通している。

### この機能をシステムで動作させるには、次の操作が必要です

* [Felix Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをダウンロードし、デプロイします。
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEM FormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [このパッケージをダウンロードして、AEM ](assets/acro-form-aem-form.zip)にインポートします。このパッケージには、acroformからXSDを作成するサンプルワークフローとHTMLページが含まれています
* [configMgr](http://localhost:4502/system/console/configMgr)を開きます。
   * 「Apache Sling Service User Mapper Service」を検索し、クリックしてプロパティを開きます。
   * `+`アイコン（プラス）をクリックして、次のサービスマッピングを追加します。
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 「保存」をクリックします。
