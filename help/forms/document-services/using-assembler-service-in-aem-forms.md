---
title: AEM Formsでの Assembler サービスの使用
description: AEM Formsでの Assembler サービスを使用した複数の pdf ファイルのアセンブリ
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 4%

---

# AEM Formsでの Assembler サービスの使用{#using-assembler-service-in-aem-forms}

この記事では、複数のPDFファイルをブラウザーにドラッグ&amp;ドロップし、組み立てられた pdf ファイルをファイルシステムに保存する機能を示すアセットを提供します。 次に、ブラウザーを使用してアップロードされた pdf ファイルをアセンブルするサーブレットのコードを示します。

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

この機能をAEM Server で動作させるには

* をダウンロードします。 [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) をローカルシステムに送信します。
* を使用してパッケージをアップロードしインストールする [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* ダウンロード[カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* ダウンロード [サービスユーザーバンドルを使用した開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* バンドルをデプロイし、 [felix web コンソール](http://localhost:4502/system/console/bundles)
* ブラウザーで次の場所を指定します。 [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* PDFファイルのファイルを 2、3 個ドラッグ&amp;ドロップします

>[!NOTE]
>
>AEM Formsのインストールが完了していることを確認します。 すべてのバンドルがアクティブ状態である必要があります。
>
>Boot delegate RSA ライブラリと BouncyCastle ライブラリが追加されていることを確認します（この節を参照）。 [AEM Formsのインストール](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**このデモの注意事項**
>
> * このコードは XFA ベースのPDFドキュメントを処理しません
>
> * 必ずPDFファイルのみをドラッグ&amp;ドロップしてください
>
>

