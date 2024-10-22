---
title: 出力サービスを使用したPDFドキュメントの生成
description: データを XDP テンプレートと結合して、非インタラクティブPDFを生成する
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 18%

---


# 出力サービスを使用したPDFドキュメントの生成

[ 出力サービス ](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) は、AEM ドキュメントサービスの一部を成す OSGi サービスです。 様々な出力フォーマットとAEM Forms Designerのデザイン機能をサポートしています。 Output サービスは、XFA テンプレートと XML データを変換して、異なる形式の印刷ドキュメントを生成します。

AEM Formsの Output サービスは、AEM Forms 6.5 のものと非常によく似ています。そのため、AEM Forms 6.5 での Output サービスの使用に詳しい場合は、AEM Formsas a Cloud Serviceのas a Cloud Serviceに移行するのは簡単です。

Output サービスを使用すると、次のことが可能なアプリケーションを作成できます。

+ テンプレートファイルに XML データを格納することで、最終形式のドキュメントを生成する。
+ 非インタラクティブ PDF、ポストスクリプト、PCL、および ZPL のプリントストリームを含む様々な形式でフォームを出力する
+ XFA フォームの PDF ファイルから印刷用 PDF を生成する。
+ 付属のテンプレートを使用して複数のデータセットを結合することにより、PDF、PostScript、PCL および ZPL の各種形式のドキュメントを一括生成する

このサービスは、AEM Formsas a Cloud Serviceインスタンスのコンテキスト内で使用するように設計されています。 次のコードスニペットは、`OutputService` を使用してサーブレットでPDFドキュメントを生成します。

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
