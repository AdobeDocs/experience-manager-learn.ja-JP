---
title: AEM FormsでのAssemblerサービスの使用
description: AEM FormsでのAssemblerサービスを使用した複数のpdfファイルのアセンブリ
feature: Assembler
version: 6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 5%

---


# AEM FormsでのAssemblerサービスの使用{#using-assembler-service-in-aem-forms}

この記事では、複数のPDFファイルをブラウザーにドラッグ&amp;ドロップし、アセンブリされたPDFファイルをファイルシステムに保存する機能を示すアセットを提供します。 次に、ブラウザーを使用してアップロードされたpdfファイルをアセンブルするサーブレットのコードを示します。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

この機能をAEM Serverで動作させるには

* [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)をローカルシステムにダウンロードします。
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用してパッケージをアップロードし、インストールします。
* [カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)をダウンロードします。
* [サービスユーザーバンドルでの開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)をダウンロードします。
* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイし、起動します。
* ブラウザーで[AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)を参照します。
* PDFファイルの2つのファイルのドラッグ&amp;ドロップ

>[!NOTE]
>
>AEM Formsのインストールが完了していることを確認します。 すべてのバンドルはアクティブ状態である必要があります。
>
>この[AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)のインストールで説明したように、- Boot delegate RSAライブラリとBouncyCastleライブラリが追加されていることを確認します。
>
>**このデモに関する注意事項**
>
> * このコードはXFAベースのPDFドキュメントを処理しません
   >
   > 
* PDFファイルのみをドラッグ&amp;ドロップしてください
>
>







