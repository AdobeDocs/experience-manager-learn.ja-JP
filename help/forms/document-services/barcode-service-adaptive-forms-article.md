---
title: アダプティブForms付きバーコードサービス
seo-title: アダプティブForms付きバーコードサービス
description: バーコードサービスを使用したバーコードのデコード、および抽出されたデータからのフォームフィールドの入力
seo-description: バーコードサービスを使用したバーコードのデコード、および抽出されたデータからのフォームフィールドの入力
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# アダプティブForms付きバーコードサービス{#barcode-service-with-adaptive-forms}

この記事では、アダプティブフォームへの入力にバーコードサービスを使用する方法を説明します。 使用例は次のとおりです。

1. バーコード付きのPDFをアダプティブフォームの添付ファイルとして追加する
1. 添付ファイルのパスがサーブレットに送信されます
1. サーブレットがバーコードをデコードし、データをJSON形式で返す
1. その後、デコードされたデータを使用してアダプティブフォームにデータが埋め込まれます

次のコードはバーコードをデコードし、デコードされた値をJSONオブジェクトに入力します。 次に、サーブレットは、呼び出し元のアプリケーションに応答してJSONオブジェクトを返します。

この機能はライブで確認できます。[サンプルポータル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)にアクセスし、「バーコードサービスのデモ」を検索してください。

```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

サーブレットコードを次に示します。 このサーブレットは、ユーザーがアダプティブフォームに添付ファイルを追加したときに呼び出されます。 サーブレットは、JSONオブジェクトを呼び出し元のアプリケーションに返します。 次に、呼び出し元のアプリケーションは、JSONオブジェクトから抽出された値をアダプティブフォームに入力します。

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }

 }

}
```

次のコードは、アダプティブフォームが参照するクライアントライブラリの一部です。 ユーザーがアダプティブフォームに添付ファイルを追加すると、このコードがトリガーされます。 コードは、リクエストパラメーターで渡された添付ファイルのパスを使用して、サーブレットにGET呼び出しを行います。 次に、サーブレット呼び出しから受信したデータを使用してアダプティブフォームに入力します。

```
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>このパッケージに含まれるアダプティブフォームは、AEM Forms6.4を使用して構築されています。このパッケージをAEM Forms6.3環境で使用する場合は、AEMフォーム6.3でアダプティブフォームを作成してください。

12行目 — サービスリゾルバーを取得するカスタムコード。 このバンドルは、この記事アセットの一部として含まれます。

23行目 — DocumentServices extractBarCodeメソッドを呼び出して、デコードされたデータを使用してJSONオブジェクトにデータを入力する

これをシステムで実行するには、次の手順に従ってください

1. [BarcodeService.](assets/barcodeservice.zip) zipをダウンロードし、パッケージマネージャーを使用してAEMに読み込みます
1. [カスタムDocumentServicesバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [DevelopingWithServiceUserバンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [サンプルPDFフォームのダウンロード](assets/barcode.pdf)
1. ブラウザーで[サンプルのアダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)を指定します。
1. 提供されたサンプルPDFのアップロード
1. フォームにデータが入力されていることが確認できます

