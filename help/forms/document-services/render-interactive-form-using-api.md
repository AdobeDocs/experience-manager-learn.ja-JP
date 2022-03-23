---
title: AEM FormsのForms Services を使用したインタラクティブPDFのレンダリング
description: AEM FormsでForms Service API を使用してインタラクティブPDFをレンダリングする
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 1%

---

# AEM FormsのForms Services を使用したインタラクティブPDFのレンダリング

AEM FormsでForms Service API を使用してインタラクティブPDFをレンダリングする

この記事では、以下のサービスについて見ていきます。

* FormsService -PDFファイルとの間でデータの書き出し/読み込みを行え、xml データを xdp テンプレートに結合してインタラクティブ pdf を生成できる、非常に汎用性の高いサービスです。

AEM Forms API の公式 Javadoc は以下のとおりです。 [ここ](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

次のコードスニペットは、FormsService の renderPDFForm 操作を使用してインタラクティブ pdf をレンダリングします。 schengen.xdp は、xml データのマージに使用されるテンプレートです。

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

行 1:xdp テンプレートを含むフォルダーの場所

行 2-4:PDFFormRenderOptions を作成し、そのプロパティを設定します

行 7:FormsService の renderPDFForm サービス操作を使用してインタラクティブPDFを生成する

行 11:生成されたインタラクティブ pdf を呼び出し元のアプリケーションに返します

**システム上のサンプルパッケージをテストするには**
1. [Felix Web コンソールを使用して、DocumentServices サンプルバンドルをダウンロードしてインストールします。](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEMパッケージマネージャーを使用して、パッケージをダウンロードしてインストールします](assets/downloadinteractivepdffrommobileform.zip)



1. [configMgr にログイン](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filter を検索します。
1. 除外されたセクションに次のパスを追加して、保存します。
1. /bin/generateinteractivepdf
1. [モバイルフォームを開く](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. いくつかのフィールドに入力し、 ***ダウンロードして入力….*** button
1. インタラクティブ pdf がローカルシステムにダウンロードされます


このサンプルパッケージには、Mobile Form に関連付けられているカスタムプロファイルが含まれています。 詳しくは、 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) ファイル。 この jsp は、モバイルフォームからデータを抽出し、にマウントされたサーブレットへのPOSTリクエストを行います。 ***/bin/generateinteractivepdf*** パス。 サーブレットは、呼び出し元のアプリケーションにインタラクティブ pdf を返します。 次に、customtoolbar.jsp のコードにより、ファイルがローカルシステムにダウンロードされます
