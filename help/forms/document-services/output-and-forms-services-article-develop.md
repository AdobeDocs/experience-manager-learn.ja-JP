---
title: AEM Forms で Output サービスや Forms サービスを使用した開発
description: AEM Forms での Output サービスおよび Forms サービス API の使用
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
duration: 152
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '552'
ht-degree: 100%

---

# AEM Forms で Output サービスや Forms サービスを使用した開発{#developing-with-output-and-forms-services-in-aem-forms}

AEM Forms での Output サービスおよび Forms サービス API の使用

この記事では、以下に注目します。

* Output サービス - 通常、 xml データを xdp テンプレートまたは PDF と結合して、統合された PDF を生成するために使用されます。詳しくは、この Output サービスの [Javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) を参照してください。
* Forms サービス - PDF ファイルとの間でデータを書き出したり読み込んだりできるようにする、非常に汎用性の高いサービスです。 詳しくは、Forms サービスの [Javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) を参照してください。


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

[AEM パッケージマネージャーを使用して、パッケージをダウンロードしてインストールします。](assets/outputandformsservice.zip)




**パッケージをインストールした後、Adobe Granite CSRF フィルターで次の URL を許可リストに加える必要があります。**

1. 上記のパスを許可リストに加えるには、下記の手順に従ってください。
1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)。
1. Adobe Granite CSRF フィルターを検索します。
1. 除外セクションに次の 3 つのパスを追加し、保存します。
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 「Sling Referrer Filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします（この設定はテスト目的でのみ使用してください）。
サンプルコードをテストする方法は多数あります。 Postman アプリを使用するのが最もすばやく簡単です。Postman を使用すると、サーバーに POST リクエストを送信できます。 システムに Postman アプリをインストールします。
アプリを起動し、次の URL を入力してデータ書き出し API をテストします。

ドロップダウンリストから「POST」を選択していることを確認してください。
http://localhost:4502/content/AemFormsSamples/exportdata.html
「認証」を必ず「基本認証」として指定してください。AEM サーバーのユーザー名とパスワードを指定します。
「本文」タブに移動し、下の画像に示すようにリクエストパラメータを指定します。
![export](assets/postexport.png)
次に「送信」ボタンをクリックします。

このパッケージには 3 つのサンプルが含まれています。 次の段落では、Output サービスまたは Forms サービスを使用するタイミング、サービスの URL、各サービスが想定する入力パラメーターについて説明します。

## データの結合と出力の統合

* Output サービスを使用してデータを xdp または PDF ドキュメントと結合し、統合された PDF を生成します。
* **POST URL**：http://localhost:4502/content/AemFormsSamples/outputservice.html
* **リクエストパラメーター：**

   * **xdp_or_pdf_file**：データの結合先となる xdp または PDF ファイル
   * **xmlfile**：xdp_or_pdf_file と結合される xml データファイル
   * **saveLocation**：レンダリングしたドキュメントをファイルシステム上に保存する場所。 例：c:\\documents\\sample.pdf

### PDF ファイルへのデータの読み込み

* Forms サービスを使用してデータを PDF ファイルに読み込みます。
* **POSTURL**：http://localhost:4502/content/AemFormsSamples/mergedata.html
* **リクエストパラメーター：**

   * **pdffile**：データの結合先の PDF ファイル
   * **xmlfile**：PDF ファイルと結合される xml データファイル
   * **saveLocation**：レンダリングしたドキュメントをファイルシステム上に保存する場所。 例：`c:\\outputsample.pdf`

**PDF ファイルからのデータの書き出し**
* Forms サービスを使用して PDF ファイルからデータを書き出します。
* **POST URL**：http://localhost:4502/content/AemFormsSamples/exportdata.html
* **リクエストパラメーター：**

   * **pdffile**：データの書き出し元の PDF ファイル
   * **saveLocation**：書き出したデータをファイルシステム上に保存する場所。例：c:\\documents\\exported_data.xml

[この Postman コレクションを読み込んで、API をテストできます。](assets/document-services-postman-collection.json)
