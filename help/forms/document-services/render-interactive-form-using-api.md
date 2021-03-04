---
title: AEM Formsの産出・Forms事業を活用した開発
seo-title: AEM Formsの産出・Forms事業を活用した開発
description: AEM FormsでのOutputとFormsサービスAPIの使用
seo-description: AEM FormsでのOutputとFormsサービスAPIの使用
feature: Forms サービス
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 2%

---


# AEM FormsのFormsサービスを使用したインタラクティブPDFのレンダリング

AEM FormsでFormsサービスAPIを使用してインタラクティブPDFをレンダリングする

この記事では、以下のサービスを見てみます。

* FormsService - PDFファイルへのデータの書き出し/読み込みを可能にし、XMLデータをxdpテンプレートに結合してインタラクティブpdfを生成する、非常に用途の広いサービスです。

AEM FormsAPIの公式Javadocは[ここ](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)に記載されています

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

1行目：xdpテンプレートが含まれるフォルダーの場所

行2 ～ 4:PDFFormRenderOptionsを作成し、そのプロパティを設定

7行目：FormsServiceのrenderPDFFormサービス操作を使用してInteractive PDFを生成します

11行目：生成されたインタラクティブpdfを呼び出し元のアプリケーションに返します

**システム上でサンプルパッケージをテストするには**
1. [Felix Web Consoleを使用したDocumentServicesサンプルバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEMパッケージマネージャーを使用してパッケージをダウンロードし、インストールします](assets/downloadinteractivepdffrommobileform.zip)



1. [configMgrにログイン](http://localhost:4502/system/console/configMgr)
1. Adobe御影石CSRFフィルタを検索
1. 除外されたセクション追加の次のパスに移動し、
1. /bin/generateinteractivepdf
1. [モバイルフォームを開く](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 2つのフィールドに入力し、***ダウンロードして入力….*** button
1. インタラクティブpdfをローカルシステムにダウンロードする必要があります


サンプルパッケージには、Mobile Formに関連付けられたカスタムプロファイルが含まれています。 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)ファイルを調べてください。 このjspは、モバイルフォームからデータを抽出し、***/bin/generateinteractivepdf***&#x200B;パスにマウントされたサーブレットにPOSTリクエストを行います。 サーブレットは、インタラクティブpdfを呼び出し元のアプリケーションに返します。 次に、customtoolbar.jspのコードは、ファイルをローカルシステムにダウンロードします


