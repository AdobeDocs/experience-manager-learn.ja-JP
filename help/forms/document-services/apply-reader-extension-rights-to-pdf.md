---
title: 使用権限を持つXDPをPDFにレンダリング
seo-title: 使用権限を持つXDPをPDFにレンダリング
description: PDFへの使用権限の適用
seo-description: PDFへの使用権限の適用
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms サービス
topic: 開発
role: Developer
level: Experienced
source-git-commit: aa90b2c1a066dc36d4ba26ecdb8b58939445ef34
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 13%

---


# Reader拡張の適用

Reader拡張機能を使用すると、PDFドキュメントの使用権限を操作できます。 使用権限は、Acrobat で使用できる機能に適用されますが、Acrobat Reader の機能には適用されません。「Reader拡張機能」で制御される機能には、ドキュメントにコメントを追加したり、フォームに入力したり、ドキュメントを保存したりする機能が含まれます。 使用権限が追加された PDF ドキュメントは、「使用権限を付与されたドキュメント」と呼ばれます。使用権限を付与された PDF ドキュメントを Adobe Reader で開いたユーザーは、そのドキュメントで有効になっている操作を実行できます。この機能をテストするには、[link](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html)を試してみます。

この使用例を達成するには、次の手順を実行する必要があります。
* [ユーザー拡張Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) 証明書を `fd-service` 追加します。

* ドキュメントに使用権限を適用するカスタムOSGiサービスを作成します。 これをおこなうコードを次に示します

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## PDFをストリーミングするサーブレットの作成 {#create-servlet-to-stream-the-pdf}

次の手順では、POSTメソッドを使用してサーブレットを作成し、Reader用の拡張PDFをユーザーに返します。 この場合、ユーザーはPDFをファイルシステムに保存するよう求められます。 これは、PDFがダイナミックPDFとしてレンダリングされ、ブラウザに付属しているPDFビューアではダイナミックPDFは処理されないためです。

次に、サーブレットのコードを示します。 サーブレットは、アダプティブフォームの&#x200B;**customsubmit**アクションから呼び出されます。
サーブレットは、UsageRightsオブジェクトを作成し、アダプティブフォームにユーザーが入力した値に基づいてプロパティを設定します。 次に、サーブレットは、この目的で作成されたサービスの**applyUsageRights**&#x200B;メソッドを呼び出します。

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;

import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];
                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);

                                        documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        documentToReaderExtend.close();
                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();
                                        documentToReaderExtend.close();
                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        int bytes;
                                        while ((bytes = fileInputStream.read()) != -1) {
                                                responseOutputStream.write(bytes);
                                        }
                                        responseOutputStream.flush();
                                        responseOutputStream.close();

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

ローカルサーバーでこれをテストするには、次の手順に従います。
1. [DevelopingWithServiceUserバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [ares.ares.core-aresバンドルをダウンロードしてインストールします](assets/ares.ares.core-ares.jar)。これには、使用権限を適用し、PDFをストリーミング戻すためのカスタムサービスとサーブレットが含まれます
1. [クライアントライブラリとカスタム送信を読み込む](assets/applyaresdemo.zip)
1. [アダプティブフォームの読み込み](assets/applyaresform.zip)
1. 「fd-service」Readerに拡張機能証明書を追加する
1. [アダプティブフォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. 適切な権限を選択し、PDFファイルをアップロードします。
1. 「送信」をクリックして、Reader拡張PDFを取得


