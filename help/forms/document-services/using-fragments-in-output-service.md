---
title: 出力サービスでのフラグメントの使用
description: crx リポジトリにあるフラグメントを使用して pdf ドキュメントを生成します
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 2%

---

# フラグメントを使用した pdf ドキュメントの生成{#developing-with-output-and-forms-services-in-aem-forms}


この記事では、xdp フラグメントを使用して pdf ファイルを生成する際に、出力サービスを使用します。 メインの xdp とフラグメントは crx リポジトリに存在します。 AEMのファイルシステムフォルダー構造を模倣することが重要です。 例えば、xdp の fragments フォルダーでフラグメントを使用する場合は、 **フラグメント** をAEMのベースフォルダーに追加します。 基本フォルダーには基本 xdp テンプレートが格納されます。 たとえば、ファイルシステムに次の構造がある場合、
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* フォルダー xdpdocuments には、基本テンプレートとフラグメントが **フラグメント** フォルダー

必要な構造は、 [フォームとドキュメント ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

次に、2 つのフラグメントを使用するサンプル xdp のフォルダー構造を示します
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Output Service — 通常、このサービスは、xdp テンプレートまたは pdf と xml データを結合して統合された pdf を生成するために使用されます。 詳しくは、 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) Output サービスの このサンプルでは、crx リポジトリ内のフラグメントを使用します。


次のコードは、フラグメントファイルにフラグメントを含めるためにPDFされました

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**システム上のサンプルパッケージをテストするには**

* [サンプル xdp ファイルをダウンロードしてAEMに読み込みます](assets/xdp-templates-fragments.zip)
* [AEMパッケージマネージャーを使用して、パッケージをダウンロードしてインストールします](assets/using-fragments-assets.zip)
* [サンプルの xdp とフラグメントは、こちらからダウンロードできます。](assets/xdptemplates.zip)

**パッケージをインストールした後、Granite CSRF FilterAdobeに次の URL をする必要がありま許可リストす。**

1. 上記のパスをするには、以下の手許可リスト順に従ってください。
1. [configMgr にログイン](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filter を検索します。
1. 除外されたセクションに次のパスを追加して、保存します。
1. /content/AemFormsSamples/usingfragments

サンプルコードをテストするには、様々な方法があります。 Postmanアプリを最もすばやく最も簡単に使用できます。 Postmanを使用すると、サーバーにPOSTリクエストを送信できます。 システムにPostmanアプリをインストールします。
アプリを起動し、次の URL を入力して、書き出しデータ API をテストします。

ドロップダウンリストhttp://localhost:4502/content/AemFormsSamples/usingfragments.htmlから「POST」を選択していることを確認します。「認証」に「基本認証」と指定してください。 AEM Server のユーザー名とパスワードの指定「本文」タブに移動し、次の画像に示すリクエストパラメーターを指定します
![書き出し](assets/using-fragment-postman.png)
次に、「送信」ボタンをクリックします。

[この Postman コレクションを読み込んで、API をテストできます](assets/usingfragments.postman_collection.json)
