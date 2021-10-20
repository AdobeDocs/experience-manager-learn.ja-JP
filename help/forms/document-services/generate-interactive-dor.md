---
title: アダプティブフォームデータを使用してインタラクティブ DoR を生成する
description: アダプティブフォームデータを XDP テンプレートと結合してダウンロード可能な PDF を生成する
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---


# インタラクティブ DoR のダウンロード

一般的な使用例としては、アダプティブフォームデータを含むインタラクティブ DoR をダウンロードできる場合が考えられます。 ダウンロードした DoR は、Adobe AcrobatまたはAdobe Readerを使用して完了します。

この使用例を達成するには、次の手順を実行する必要があります

## XDP のサンプルデータの生成

* XDP をAEM Forms Designer で開きます。
* ファイルをクリック |フォームのプロパティ |プレビュー
* 「プレビューデータを生成」をクリックします。
* 「生成」をクリックします。
* 「form-data.xml」など、意味のあるファイル名を指定する

## xml データから XSD を生成

無料のオンラインツールを使用して、 [XSD を生成](https://www.freeformatter.com/xsd-generator.html) を、前の手順で生成した xml データから。

## アダプティブフォームの作成

前の手順の XSD に基づいてアダプティブフォームを作成します。 クライアントライブラリ「irs」を使用するようにフォームを関連付けます。 このクライアントライブラリには、呼び出し元のアプリケーションにPDFを返すサーブレットにPOST呼び出しを行うコードが含まれています。次のコードは、 _ダウンロードPDF_ がクリックされた

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## カスタムサーブレットの作成

データを XDP テンプレートと結合し、PDF を返すカスタムサーブレットを作成します。 これをおこなうコードを次に示します。 カスタムサーブレットは、 [AEMformsDocumentServices.core-1.0-SNAPSHOT バンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)) をクリックします。

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

このサンプルコードでは、テンプレート名 (f8918-r14e_redo-barcode_3 2.xdp) はハードコードされています。 テンプレート名をサーブレットに簡単に渡して、このコードをすべてのテンプレートに対して汎用的に機能させることができます。


## サーバーにサンプルをデプロイします。

ローカルサーバーでこれをテストするには、次の手順に従います。

1. [DevelopingWithServiceUser バンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 次のエントリを Apache Sling Service User Mapper Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service に追加します。
1. [カスタム DocumentServices バンドルをダウンロードしてインストールする](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). このサーブレットは、データを XDP テンプレートとマージし、PDF をストリーミングして戻す機能を持ちます
1. [クライアントライブラリの読み込み](assets/irs.zip)
1. [アダプティブフォームの読み込み](assets/f8918complete.zip)
1. [XDP テンプレートとスキーマの読み込み](assets/xdp-template-and-xsd.zip)
1. [アダプティブフォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. フォームフィールドの一部に入力します。
1. 「ダウンロードPDF」をクリックして、PDFを取得します。 PDFのダウンロードが完了するまで、数秒待つ必要がある場合があります


