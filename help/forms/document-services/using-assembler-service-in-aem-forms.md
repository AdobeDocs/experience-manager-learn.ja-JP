---
title: AEM FormsでのAssemblerサービスの使用
seo-title: AEM FormsでのAssemblerサービスの使用
description: AEM FormsでのAssemblerサービスを使用した複数のpdfファイルのアセンブリ
seo-description: AEM FormsでのAssemblerサービスを使用した複数のpdfファイルのアセンブリ
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: Assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 5%

---


# AEM Forms{#using-assembler-service-in-aem-forms}でのAssemblerサービスの使用

この記事では、複数のPDFファイルをブラウザーにドラッグ&amp;ドロップし、アセンブリされたPDFファイルをファイルシステムに保存する機能を示すアセットを提供します。 以下は、ブラウザーを使用してアップロードされたpdfファイルをアセンブルするサーブレットのコードです。

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

この機能をAEMサーバーで動作させるには

* [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)をローカルシステムにダウンロードします。
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、パッケージをアップロードしてインストールします
* [カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)をダウンロード
* [サービスユーザーバンドルで開発中](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)のダウンロード
* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイおよび開始します。
* ブラウザーに[AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)を指定します。
* 2つのPDFファイルをドラッグ&amp;ドロップする

>[!NOTE]
>
>AEM Formsのインストールが完了していることを確認します。 すべてのバンドルがアクティブ状態になっている必要があります。
>
>「[AEM Formsのインストール](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)」に記載されているように、ブート委任RSAライブラリとBouncyCastleライブラリが追加されていることを確認します。
>
>**このデモに関する注意事項**
>
> * コードはXFAベースのPDFドキュメントを処理しません
   >
   > 
* PDFファイルのみをドラッグ&amp;ドロップしてください
>
>







