---
title: AEM FormsでのOutputおよびForms Servicesを使用した開発
seo-title: AEM FormsでのOutputおよびForms Servicesを使用した開発
description: AEM FormsでのOutputおよびForms Service APIの使用
seo-description: AEM FormsでのOutputおよびForms Service APIの使用
feature: Forms サービス
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---


# AEM FormsでForms Servicesを使用したインタラクティブPDFのレンダリング

AEM FormsでFormsサービスAPIを使用してインタラクティブPDFをレンダリングする

この記事では、以下のサービスについて見ていきます。

* FormsService - PDFファイルとの間でデータの書き出し/読み込みを行い、xmlデータをxdpテンプレートに結合してインタラクティブpdfを生成する、非常に汎用性の高いサービスです。

AEM Forms APIの公式Javadocは、[こちら](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)を参照してください。

次のコードスニペットは、FormsServiceのrenderPDFForm操作を使用してインタラクティブpdfをレンダリングします。 schengen.xdpは、xmlデータのマージに使用されるテンプレートです。

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

1行目：xdpテンプレートを含むフォルダーの場所

行2 ～ 4:PDFFormRenderOptionsを作成し、そのプロパティを設定します

7行目：FormsServiceのrenderPDFFormサービス操作を使用してインタラクティブPDFを生成します

11行目：生成されたインタラクティブpdfを呼び出し元のアプリケーションに返します

**システム上でサンプルパッケージをテストするには**
1. [Felix Webコンソールを使用してDocumentServicesサンプルバンドルをダウンロードし、インストールします。](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEMパッケージマネージャーを使用して、パッケージをダウンロードしてインストールします](assets/downloadinteractivepdffrommobileform.zip)



1. [configMgrにログインします。](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filterを検索します。
1. 除外されたセクションに次のパスを追加し、保存します。
1. /bin/generateinteractivepdf
1. [モバイルフォームを開く](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 2つのフィールドに入力し、「***ダウンロードして入力…」をクリックします。.*** button
1. インタラクティブpdfがローカルシステムにダウンロードされます


サンプルパッケージには、Mobile Formに関連付けられたカスタムプロファイルが含まれています。 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)ファイルを参照してください。 このjspは、モバイルフォームからデータを抽出し、***/bin/generateinteractivepdf***&#x200B;パスにマウントされたサーブレットに対してPOSTリクエストを実行します。 サーブレットは、呼び出し元のアプリケーションにインタラクティブpdfを返します。 customtoolbar.jspのコードは、ファイルをローカルシステムにダウンロードします


