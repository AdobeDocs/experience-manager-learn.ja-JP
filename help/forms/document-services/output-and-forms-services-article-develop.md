---
title: AEM FormsでのOutputおよびForms Servicesを使用した開発
seo-title: AEM FormsでのOutputおよびForms Servicesを使用した開発
description: AEM FormsでのOutputおよびForms Service APIの使用
seo-description: AEM FormsでのOutputおよびForms Service APIの使用
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: Output サービス
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: 開発
role: Developer
level: Intermediate
source-git-commit: 67be45dbd72a8af8b9ab60452ff15081c6f9f192
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 2%

---


# AEM FormsでのOutputおよびForms Servicesを使用した開発{#developing-with-output-and-forms-services-in-aem-forms}

AEM FormsでのOutputおよびForms Service APIの使用

この記事では、以下を見てみます。

* Output Service — 通常、このサービスは、xmlデータをxdpテンプレートまたはpdfと結合して統合されたpdfを生成するために使用されます。 詳しくは、Outputサービスの[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)を参照してください。
* FormsService - PDFファイルとの間でデータの書き出し/読み込みを行う、非常に汎用性の高いサービスです。 詳しくは、 Formsサービスの[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html)を参照してください。


次のコードスニペットは、PDFファイルからデータを書き出します

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

1行目は、リクエストからpdffileを抽出します

行2は、リクエストからsaveLocationを抽出します

5行目はFormsServiceを保持します

6行目はPDFファイルからxmlDataを書き出します

**システム上でサンプルパッケージをテストするには**

[AEMパッケージマネージャーを使用して、パッケージをダウンロードしてインストールします](assets/outputandformsservice.zip)




**パッケージをインストールした後、Granite CSRFフ許可リストィルターのAdobeに次のURLをする必要があります。**

1. 上記のパスをするには、次の手許可リスト順に従ってください。
1. [configMgrにログインします。](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filterを検索します。
1. 除外されたセクションに次の3つのパスを追加し、保存します。
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 「Sling Referrer filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします。 （この設定はテスト目的でのみ使用します）。
サンプルコードをテストする方法はいくつかあります。 Postmanアプリを使用するのが最も簡単です。 Postmanを使用すると、サーバーにPOSTリクエストを送信できます。 システムにPostmanアプリをインストールします。
デスクトップアプリケーションを起動し、次のURLを入力して書き出しデータAPIをテストします。

ドロップダウンリストから「POST」を選択していることを確認します。
http://localhost:4502/content/AemFormsSamples/exportdata.html
「認証」を「基本認証」として指定してください。 AEM Serverのユーザー名とパスワードの指定
「Body」タブに移動し、次の図に示すリクエストパラメーターを指定します
![export](assets/postexport.png)
次に、「送信」ボタンをクリックします。

このパッケージには3つのサンプルが含まれています。 次の段落では、OutputサービスまたはFormsサービスを使用するタイミング、サービスのURL、各サービスが想定する入力パラメーターについて説明します

**データの結合と出力の統合：**

* Outputサービスを使用して、データをxdpまたはpdfドキュメントと結合し、統合されたpdfを生成する
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **要求パラメーター -**

   * xdp_or_pdf_file :データを結合するxdpまたはpdfファイル
   * xmlfile:xdp_or_pdf_fileとマージされるxmlデータファイル
   * saveLocation:レンダリングされたドキュメントをファイルシステム上に保存する場所

**データをPDFファイルに読み込む：**
* FormsServiceを使用したPDFファイルへのデータの読み込み
* **POSTURL**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **要求パラメーター:**

   * pdffile :データをマージするPDFファイル
   * xmlfile:PDFファイルとマージされるXMLデータファイル
   * saveLocation:レンダリングされたドキュメントをファイルシステム上に保存する場所。 例えば、c:\\\outputsample.pdfのように指定します。

**PDFファイルからのデータの書き出し**
* FormsServiceを使用したPDFファイルからのデータの書き出し
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **要求パラメーター:**

   * pdffile :データの書き出し元のpdfファイル
   * saveLocation:ファイルシステム上で書き出したデータを保存する場所

[このPostmanコレクションを読み込んで、APIをテストできます。](assets/document-services-postman-collection.json)

