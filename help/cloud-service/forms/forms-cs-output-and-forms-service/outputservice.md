---
title: Output サービスを使用した PDF ドキュメントの生成
description: XDP テンプレートを使用してデータを結合し、非インタラクティブ PDF を生成します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 100%

---

# Output サービスを使用した PDF ドキュメントの生成

[Output サービス](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html)は、AEM ドキュメントサービスの一部である OSGi サービスです。Output サービスは、様々な出力形式や、AEM Forms Designer の出力設計機能をサポートしています。Output サービスでは、XFA テンプレートと XML データを変換して、様々な形式の印刷ドキュメントを生成します。

AEM Forms as a Cloud Service の Output サービスは AEM Forms 6.5 の Output サービスとよく似ているので、AEM Forms 6.5 の Output サービスの使用に慣れている場合、AEM Forms as a Cloud Service への移行は簡単です。

Output サービスを使用すると、次を可能にするアプリケーションを作成できます。

+ テンプレートファイルに XML データを格納することで、最終形式のドキュメントを生成する。
+ 非インタラクティブ PDF、ポストスクリプト、PCL、および ZPL のプリントストリームを含む様々な形式でフォームを出力する
+ XFA フォームの PDF ファイルから印刷用 PDF を生成する。
+ 付属のテンプレートを用いて複数のデータセットを結合することにより、PDF、PostScript、PCL および ZPL の形式のドキュメントを一括生成する。

このサービスは、AEM Forms as a Cloud Service インスタンスのコンテキスト内で使用するように設計されています。次のコードスニペットでは、`OutputService` を使用してサーブレット内に PDF ドキュメントを生成します。

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
