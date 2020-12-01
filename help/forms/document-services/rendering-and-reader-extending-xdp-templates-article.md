---
title: 使用権限を持つPDFへのXDPのレンダリング
seo-title: 使用権限を持つPDFへのXDPのレンダリング
description: 使用権限をPDFに適用
seo-description: 使用権限をPDFに適用
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: forms-service
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---


# 使用権限{#rendering-xdp-into-pdf-with-usage-rights}を持つXDPをPDFにレンダリング

一般的な使用例は、xdpをPDFにレンダリングし、レンダリングされたPDFにReader拡張を適用する場合です。

例えば、AEM Formsのフォームポータルで、ユーザーがXDPをクリックすると、XDPをPDFとしてレンダリングし、ReaderでPDFを拡張できます。

この機能をテストするには、[リンク](https://forms.enablementadobe.com/content/samples/samples.html?query=0)を試してみてください。 サンプル名は「Render XDP with RE」です。

この使用例を達成するには、次の作業を行う必要があります。

* Reader追加拡張証明書を&quot;fd-service&quot;ユーザーに送信する必要があります。 Reader拡張機能証明書を追加する手順は、[ここ](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)に一覧表示されます

* 使用権限をレンダリングして適用するカスタムOSGiサービスを作成します。 これを達成するコードを次に示します。

## Render XDP and Apply usage rights {#render-xdp-and-apply-usage-rights}

* 7行目：FormsServiceのrenderPDFFormを使用して、XDPからPDFを生成します。

* 行8 ～ 14:適切な使用権限が設定されます。 これらの使用権限は、OSGi設定から取得されます。

* 20行目：サービスユーザーfd-serviceに関連付けられたリソースリゾルバーを使用します。

* 24行目：使用権限の適用には、DocumentAssuranceServiceのsecureDocumentメソッドを使用します

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

次のスクリーンショットは、公開された設定プロパティを示しています。 一般的な使用権限のほとんどは、この設定によって公開されます。

![](assets/configurationproperties.gif)

次のコードは、OSGi設定を構築するために使用するコードを示します

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## PDFをストリーミングするサーブレットの作成{#create-servlet-to-stream-the-pdf}

次の手順は、GETメソッドを使用してサーブレットを作成し、Reader用の拡張PDFをユーザーに返すことです。 この場合、PDFをファイルシステムに保存するように求められます。 これは、PDFがダイナミックPDFとしてレンダリングされ、ブラウザに付属するPDFビューアではダイナミックPDFは処理されないためです。

サーブレットのコードを次に示します。 CRXリポジトリのXDPのパスをこのサーブレットに渡します。

次に、com.aemformssamples.documentservices.core.DocumentServicesのrenderAndExtendXdpメソッドを呼び出します。

その後、Reader Extended PDFが呼び出し元のアプリケーションにストリーミングされます

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

ローカルサーバーでテストするには、次の手順に従ってください
1. [DevelopingWithServiceUserバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [AEMFormsDocumentServicesバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Package Managerを使用して、この記事に関連するアセットをAEMにダウンロードして読み込みます](assets/renderandextendxdp.zip)
   * このパッケージにはサンプルポータルとxdpファイルが含まれています
1. Reader拡張追加証明書を&quot;fd-service&quot;ユーザーに送信
1. ブラウザーで[ポータルWebページ](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)を指定します。
1. pdfアイコンをクリックしてxdpをレンダリングし、Reader拡張のpdfを取得します



