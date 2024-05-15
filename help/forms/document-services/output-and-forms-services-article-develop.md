---
title: AEM Forms で Output サービスや Forms サービスを使用した開発
description: AEM Forms での Output および Forms Service API を使用した開発について説明します。
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 100%

---

# AEM Forms で Output サービスや Forms サービスを使用した開発{#developing-with-output-and-forms-services-in-aem-forms}

AEM Forms での Output および Forms Service API を使用した開発について説明します。

この記事では、次に注目します。

* [Output サービス](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) - 通常、このサービスは xml データを xdp テンプレートまたは PDF と結合して、フラット化された PDF の生成に使用されます。
* [Forms サービス](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) - これは、xdp を PDF としてレンダリングしたり、PDF ファイルとの間でデータを書き出したり読み込んだりできるようにする、汎用性の高いサービスです。


次のコードスニペットは、データを PDF ファイルから書き出しています。

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

行 1 ではリクエストから pdffile を抽出します。

行 2 ではリクエストから saveLocation を抽出します。

行 5 では FormsService を取得します。

行 6 では PDF ファイルから xmlData を書き出します。

**システム上のサンプルパッケージをテストするには：**

[AEM パッケージマネージャーを使用して、パッケージをダウンロードしてインストールします。](assets/using-output-and-form-service-api.zip)




**パッケージをインストールした後、Adobe Granite CSRF フィルターで次の URL を許可リストに加える必要があります。**

1. 上記のパスを許可リストに加えるには、下記の手順に従ってください。
1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)。
1. Adobe Granite CSRF フィルターを検索します。
1. 除外セクションに次の 3 つのパスを追加し、保存します。
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. 「Sling Referrer Filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします（この設定は、テスト目的でのみ使用する必要があります）

## サンプルのテスト

サンプルコードをテストするには、様々な方法があります。Postman アプリを使用するのが最もすばやく簡単です。Postman を使用すると、サーバーに POST リクエストを送信できます。

* システムに Postman アプリをインストールします。
* アプリを起動し、適切な URL を入力します。
* ドロップダウンリストから「POST」を選択していることを確認します。
* 「認証」を「Basic Auth」として指定してください。AEM サーバーのユーザー名とパスワードを指定します。
* 「本文」タブでリクエストパラメーターを指定します。
* 「送信」ボタンをクリックします。

このパッケージには 4 つのサンプルが含まれています。次の段落では、Output サービスまたは Forms サービスを使用するタイミング、サービスの URL、各サービスが想定する入力パラメーターについて説明します。

## OutputService を使用した xdp テンプレートとのデータの結合

* Output サービスを使用してデータを xdp または PDF ドキュメントと結合し、統合された PDF を生成します。
* **POST URL**：http://localhost:4502/content/AemFormsSamples/outputservice.html
* **リクエストパラメーター：**

   * **xdp_or_pdf_file**：データの結合先となる xdp または PDF ファイル
   * **xmlfile**：xdp_or_pdf_file と結合される xml データファイル
   * **saveLocation**：レンダリングしたドキュメントをファイルシステム上に保存する場所。 例：c:\\documents\\sample.pdf

### FormsService API の使用

#### データの読み込み

* FormsService importData を使用したデータの PDF ファイルへの読み込み
* **POSTURL**：http://localhost:4502/content/AemFormsSamples/mergedata.html

* **リクエストパラメーター：**

   * **pdffile**：データの結合先の PDF ファイル
   * **xmlfile**：PDF ファイルと結合される xml データファイル
   * **saveLocation**：レンダリングしたドキュメントをファイルシステム上に保存する場所。 例：`c:\\outputsample.pdf`

#### データの書き出し

* FormsService exportData API を使用した PDF ファイルからのデータの書き出し
* **POST URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **リクエストパラメーター：**

   * **pdffile**：データの書き出し元の PDF ファイル
   * **saveLocation**：書き出したデータをファイルシステム上に保存する場所。例：c:\\documents\\exported_data.xml

#### XDP のレンダリング

* 静的または動的 PDF としての XDP テンプレートのレンダリング
* FormsService renderPDFForm API を使用した PDF としての xdp テンプレートのレンダリング
* **POST URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* リクエストパラメーター：
   * xdpName：PDF としてレンダリングする xdp ファイルの名前

[この Postman コレクションを読み込んで、API をテストできます。](assets/UsingDocumentServicesInAEMForms.postman_collection.json)
