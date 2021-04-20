---
title: AEM Formsの産出・Forms事業を活用した開発
seo-title: AEM Formsの産出・Forms事業を活用した開発
description: AEM FormsでのOutputとFormsサービスAPIの使用
seo-description: AEM FormsでのOutputとFormsサービスAPIの使用
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
translation-type: tm+mt
source-git-commit: 67be45dbd72a8af8b9ab60452ff15081c6f9f192
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 2%

---


# AEM FormsのOutputとForms・サービスを使った開発{#developing-with-output-and-forms-services-in-aem-forms}

AEM FormsでのOutputとFormsサービスAPIの使用

この記事では、以下を見てみます。

* Outputサービス — 通常、このサービスは、xmlデータをxdpテンプレートまたはpdfにマージして統合済みpdfを生成する際に使用されます。 詳しくは、Outputサービスの[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)を参照してください。
* FormsService - PDFファイルへのデータの書き出しと読み込みを可能にする、用途の広いサービスです。 詳しくは、Formsサービスの[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html)を参照してください。


次のコードスニペットは、PDFファイルからデータを書き出します

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

行1は、リクエストからpdffileを抽出します

行2は、リクエストからsaveLocationを抽出します

5行目はFormsServiceを保持します

6行目は、PDFファイルからxmlDataを書き出します

**システム上でサンプルパッケージをテストするには**

[AEMパッケージマネージャーを使用してパッケージをダウンロードし、インストールします](assets/outputandformsservice.zip)




**パッケージのインストール後、AdobeGranite CSRF Filterで許可リスト次のURLをする必要があります。**

1. 上記のパスを許可リストするには、次の手順に従ってください。
1. [configMgrにログイン](http://localhost:4502/system/console/configMgr)
1. Adobe御影石CSRFフィルタを検索
1. 除外されたセクション追加の次の3つのパス。
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 「Sling転送者フィルター」を検索します
1. 「空白を許可」チェックボックスをオンにします。 （この設定はテスト目的でのみ使用します）。
サンプルコードをテストする方法はいくつかあります。 最も簡単で迅速な方法は、Postmanアプリを使用することです。 Postmanは、サーバーにPOSTリクエストを行うことを許可します。 システムにPostmanアプリをインストールします。
アプリを起動し、次のURLを入力してExport Data APIをテストします

ドロップダウンリストから「POST」を選択したことを確認します
http://localhost:4502/content/AemFormsSamples/exportdata.html
「認証」には「基本認証」を指定してください。 AEM Serverのユーザー名とパスワードを指定します
「Body」タブに移動し、次の画像に示すようにリクエストパラメーターを指定します
![エクスポート](assets/postexport.png)
次に、「送信」ボタンをクリックします

パッケージには3つのサンプルが含まれています。 次の段落では、OutputサービスまたはFormsサービスを使用するタイミング、サービスのURL、各サービスが想定する入力パラメーターについて説明します

**データと出力の統合：**

* Outputサービスを使用して、データをxdpまたはpdfドキュメントとマージし、統合されたpdfを生成する
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **要求パラメーター -**

   * xdp_or_pdf_file :データとマージするxdpまたはpdfファイル
   * xmlfile:xdp_or_pdf_fileとマージされるxmlデータファイル
   * saveLocation:レンダリングされたドキュメントをファイルシステムに保存する場所

**PDFファイルにデータを読み込む：**
* FormsServiceを使用したPDFファイルへのデータの読み込み
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **要求パラメーター:**

   * pdffile :データとマージするpdfファイル
   * xmlfile:PDFファイルとマージされるXMLデータファイル
   * saveLocation:レンダリングされたドキュメントをファイルシステムに保存する場所。 例：c:\\\outputsample.pdf

**PDFファイルからのデータの書き出し**
* FormsServiceを使用したPDFファイルからのデータの書き出し
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **要求パラメーター:**

   * pdffile :データの書き出し元のpdfファイル
   * saveLocation:ファイルシステム上に書き出したデータを保存する場所

[このpostmanコレクションを読み込んでAPIをテストできます](assets/document-services-postman-collection.json)

