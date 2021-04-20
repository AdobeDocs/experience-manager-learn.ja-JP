---
title: AEM FormsとのAcroforms
description: Acroformを使用したアダプティブフォームの作成、データの結合、PDFの取得に関する手順を説明するチュートリアルです。 結合されたデータを含むPDFを、Adobe Signを使用して署名用に送信できます。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 4%

---


# AcroformsからのアダプティブFormsの作成

組織は様々なフォームを持っています。 これらのフォームの一部はMicrosoft Wordで作成され、PDFに変換されます。 これらのフォームは、デフォルトでは、Adobe ReaderまたはAcrobatを使用して入力することはできません。 AcrobatやReaderを使用して入力可能なフォームにするには、これらのフォームをAcroformに変換する必要があります。 Acroformsは、Acrobatを使用して作成されたフォームです。 この記事では、Acroformからアダプティブフォームを作成し、データをAcroformにマージしてPDFを取得する手順を説明します。 結合されたデータを含むPDFは、Adobe Signを使用して署名用に送信することもできます。

>[!NOTE]
>
>AEM Forms6.5を使用している場合は、Automated forms conversion機能を使用してください。

## 前提条件

* AEM Forms6.3または6.4のインストールおよび設定
* Adobe Acrobatへのアクセス
* AEM/AEM Formsに精通している。

### この機能をシステムで動作させるには、以下が必要です。

* [Felix Web Console](http://localhost:4502/system/console/bundles)を使用してバンドルをダウンロードし、デプロイします
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [このパッケージをダウンロードし、AEMにインポートします](assets/acro-form-aem-form.zip)。このパッケージには、acroformからXSDを作成するためのサンプルワークフローとhtmlページが含まれています
* [configMgr](http://localhost:4502/system/console/configMgr)を開きます。
   * 「Apache Sling Service User Mapper Service」を検索し、をクリックしてプロパティを開きます
   * `+`アイコン（プラス）をクリックして、次のサービスマッピングを追加します
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 「保存」をクリックします
