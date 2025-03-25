---
title: アダプティブフォームを使用したバーコードサービス
description: バーコードサービスを使用してバーコードをデコードし、抽出したデータからフォームフィールドを設定します。
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 100%

---

# アダプティブフォームを使用したバーコードサービス{#barcode-service-with-adaptive-forms}

この記事では、アダプティブフォームに入力するためのバーコードサービスの使用方法を紹介します。 ユースケースを次に示します。

1. ユーザーが、PDF をアダプティブフォームの添付ファイルとして追加します
1. 添付ファイルのパスがサーブレットに送信されます
1. サーブレットがバーコードをデコードし、データを JSON 形式で返します
1. 次に、デコードされたデータを使用してアダプティブフォームに値が入力されます

次のコードは、バーコードをデコードし、JSON オブジェクトにデコードされた値を入力します。 次に、サーブレットは、呼び出し元のアプリケーションに対する応答で JSON オブジェクトを返します。



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

次に、サーブレットコードを示します。 このサーブレットは、ユーザーがアダプティブフォームに添付ファイルを追加すると呼び出されます。 サーブレットは、JSON オブジェクトを呼び出し元のアプリケーションに戻します。 次に、呼び出し元のアプリケーションが、JSON オブジェクトから抽出された値をアダプティブフォームに入力します。

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

次のコードは、アダプティブフォームが参照するクライアントライブラリの一部です。 ユーザーがアダプティブフォームに添付ファイルを追加すると、このコードがトリガーされます。 コードは、リクエストパラメーターで渡された添付ファイルのパスを使用して、サーブレットに対して GET 呼び出しを行います。サーブレット呼び出しから受け取ったデータは、アダプティブフォームへの入力に使用されます。

```javascript
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
>このパッケージに含まれるアダプティブフォームは、AEM Forms 6.4 を使用して作成されています。このパッケージを AEM Forms 6.3 環境で使用する場合は、AEM Forms 6.3 でアダプティブフォームを作成してください。

12 行目：サービスリゾルバーを取得するカスタムコード。 このバンドルは、この記事のアセットの一部として含まれています。

23 行目 - DocumentServices extractBarCode メソッドを呼び出して、デコードされたデータを含む JSON オブジェクトを取得します

これをシステムで実行するには、次の手順に従ってください。

1. [BarcodeService.zip をダウンロード](assets/barcodeservice.zip)して、パッケージマネージャーを使用して AEM に読み込みます
1. [カスタムドキュメントサービスバンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [DevelopingWithServiceUser バンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [サンプル PDF フォームをダウンロードします](assets/barcode.pdf)
1. ブラウザーで[サンプルアダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)を開きます
1. 提供されたサンプル PDF をアップロードします
1. フォームにデータが入力されていることを確認できます
