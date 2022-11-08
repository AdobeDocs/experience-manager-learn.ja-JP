---
title: AEM Formsでの Output およびForms Services を使用した開発
description: AEM Formsでの Output およびForms Service API の使用
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 2%

---

# AEM Formsでの Output およびForms Services を使用した開発{#developing-with-output-and-forms-services-in-aem-forms}

AEM Formsでの Output およびForms Service API の使用

この記事では、以下を見ていきます

* Output Service — 通常、このサービスは、xdp テンプレートまたは pdf と xml データを結合して統合された pdf を生成するために使用されます。 詳しくは、こちらを参照してください。 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) Output サービスの
* FormsService — これは、データの書き出しとPDFファイルへの読み込みを可能にする、非常に汎用性の高いサービスです。 詳しくは、こちらを参照してください。 [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) Formsサービスの


次のコードスニペットは、データをPDFファイルから書き出します

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

行 1 は、リクエストから pdffile を抽出します

Line2 は、リクエストから saveLocation を抽出します

5 行目は FormsService を保持します

6 行目は xmlData をPDF・ファイルからエクスポート

**システム上のサンプルパッケージをテストするには**

[AEMパッケージマネージャーを使用して、パッケージをダウンロードしてインストールします](assets/outputandformsservice.zip)




**パッケージをインストールした後、Granite CSRF FilterAdobeに次の URL をする必要がありま許可リストす。**

1. 上記のパスをするには、以下の手許可リスト順に従ってください。
1. [configMgr にログイン](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filter を検索します。
1. 除外されたセクションに次の 3 つのパスを追加し、保存します。
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 「Sling Referrer filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします。 （この設定はテスト目的でのみ使用する必要があります）サンプルコードをテストする方法は多数あります。 Postmanアプリを最もすばやく最も簡単に使用できます。 Postmanを使用すると、サーバーにPOSTリクエストを送信できます。 システムにPostmanアプリをインストールします。
アプリを起動し、次の URL を入力して、書き出しデータ API をテストします。

ドロップダウンリストhttp://localhost:4502/content/AemFormsSamples/exportdata.htmlから「POST」を選択していることを確認します。「認証」に「基本認証」と指定してください。 AEM Server のユーザー名とパスワードの指定「本文」タブに移動し、次の画像に示すリクエストパラメーターを指定します
![書き出し](assets/postexport.png)
次に、「送信」ボタンをクリックします。

このパッケージには 3 つのサンプルが含まれています。 次の段落では、Output サービスまたはForms Service を使用するタイミング、サービスの URL、各サービスが想定する入力パラメーターについて説明します

## データの結合と出力の統合

* Output Service を使用して xdp または pdf ドキュメントとデータを結合し、統合された pdf を生成します
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **要求パラメーター -**

   * **xdp_or_pdf_file** :データの結合先となる xdp または pdf ファイル
   * **xmlfile**:xdp_or_pdf_file とマージされる xml データファイル
   * **saveLocation**:レンダリングしたドキュメントをファイルシステム上に保存する場所。 例： c:\\documents\\sample.pdf

### データをPDFファイルにインポート

* FormsService を使用したデータのPDFファイルへの読み込み
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **要求パラメーター:**

   * **pdffile** :データの結合先の pdf ファイル
   * **xmlfile**:PDF ファイルとマージされる xml データファイル
   * **saveLocation**:レンダリングしたドキュメントをファイルシステム上に保存する場所。 例えば、`c:\\outputsample.pdf` です。

**データをPDFファイルから書き出し**
* FormsService を使用してデータをPDFファイルから書き出す
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **要求パラメーター:**

   * **pdffile** :データの書き出し元の pdf ファイル
   * **saveLocation**:エクスポートしたデータをファイルシステムに保存する場所。 例： c:\\documents\\exported_data.xml

[この Postman コレクションを読み込んで、API をテストできます](assets/document-services-postman-collection.json)
