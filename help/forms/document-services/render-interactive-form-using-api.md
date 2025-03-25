---
title: AEM Forms の Forms Services を使用したインタラクティブ PDF のレンダリング
description: AEM Forms で Forms Service API を使用してインタラクティブ PDF をレンダリングする
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 100%

---

# AEM Forms の Forms Services を使用したインタラクティブ PDF のレンダリング

AEM Forms で Forms Service API を使用してインタラクティブ PDF をレンダリングする

この記事では、次のサービスに注目します。

* FormsService - PDF ファイルとの間でデータの書き出し／読み込みを行えると共に、xml データを xdp テンプレートに結合してインタラクティブ PDF を生成できるようにする、非常に汎用性の高いサービスです。

AEM Forms API の公式の javadoc については、[こちら](https://helpx.adobe.com/jp/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)を参照してください。

次のコードスニペットでは、FormsService の renderPDFForm 操作を使用してインタラクティブ PDF をレンダリングしています。schengen.xdp は、xml データの結合に使用されているテンプレートです。

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

1 行目：xdp テンプレートを含んだフォルダーの場所

2～4 行目：PDFFormRenderOptions を作成し、そのプロパティを設定します。

7 行目：FormsService の renderPDFForm サービス操作を使用して、インタラクティブ PDF を生成します。

11 行目：生成されたインタラクティブ PDF を呼び出し元のアプリケーションに返します

**システム上のサンプルパッケージをテストするには：**
1. [DevelopingWithServiceUserBundle をダウンロードしてインストールします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Felix web コンソールを使用して、DocumentServices サンプルバンドルをダウンロードしてインストールします。](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEM パッケージマネージャーを使用して、パッケージをダウンロードしてインストールします。](assets/downloadinteractivepdffrommobileform.zip)

1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)。
1. Adobe Granite CSRF フィルターを検索します。
1. 除外されたセクションに次のパスを追加して、保存します。
1. /bin/generateinteractivepdf
1. _Apache Sling Service User Mapper Service_ を探し、クリックしてプロパティを開きます。
   1. *+* アイコン（プラス）をクリックして、次のサービスマッピングを追加します。
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 「保存」をクリック
1. [モバイルフォームを開く](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. いくつかのフィールドに入力したあと、「***ダウンロードして入力***」ボタンをクリックします
1. インタラクティブ PDF がローカルシステムにダウンロードされます。


サンプルパッケージには、モバイルフォームに関連付けられたカスタムプロファイルが含まれています。[customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) ファイルを調べてください。この jsp では、モバイルフォームからデータを抽出し、***/bin/generateinteractivepdf*** パスにマウントされているサーブレットに POST リクエストを行います。このサーブレットは、呼び出し元のアプリケーションにインタラクティブ PDF を返します。次に、customtoolbar.jsp のコードにより、ファイルがローカルシステムにダウンロードされます。
