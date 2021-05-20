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
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---


# 使用権限{#rendering-xdp-into-pdf-with-usage-rights}を使用してXDPをPDFにレンダリング

一般的な使用例は、xdpをPDFにレンダリングし、レンダリングされたPDFにReader拡張を適用する場合です。

例えば、AEM Formsのフォームポータルで、ユーザーがXDPをクリックすると、XDPをPDFとしてレンダリングし、ReaderでPDFを拡張できます。

この機能をテストするには、[link](https://forms.enablementadobe.com/content/samples/samples.html?query=0)を試してみます。 サンプル名は「Render XDP with RE」です。

この使用例を達成するには、次の手順を実行する必要があります。

* Reader拡張証明書を「fd-service」ユーザーに追加します。 ReaderExtensions証明書を追加する手順は、[ここ](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)に記載されています。

* 使用権限をレンダリングして適用するカスタムOSGiサービスを作成します。 これをおこなうコードを次に示します

## XDPのレンダリングと使用権限の適用{#render-xdp-and-apply-usage-rights}

* 7行目：FormsServiceのrenderPDFFormを使用して、XDPからPDFを生成します。

* 8～14行目：適切な使用権限が設定されます。 これらの使用権限はOSGi設定から取得されます。

* 20行目：サービスユーザーfd-serviceに関連付けられたresourceresolverを使用します。

* 24行目：使用権限の適用には、DocumentAssuranceServiceのsecureDocumentメソッドが使用されます

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

次のスクリーンショットは、公開された設定プロパティを示しています。 一般的な使用権限のほとんどは、この設定を通じて公開されます。

![](assets/configurationproperties.gif)

次のコードは、OSGi設定の構築に使用するコードを示しています

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

## PDFをストリーミングするサーブレットを作成{#create-servlet-to-stream-the-pdf}

次の手順では、GETメソッドを使用してサーブレットを作成し、リーダー用の拡張PDFをユーザーに返します。 この場合、ユーザーはPDFをファイルシステムに保存するよう求められます。 これは、PDFがダイナミックPDFとしてレンダリングされ、ブラウザに付属しているPDFビューアではダイナミックPDFは処理されないためです。

次に、サーブレットのコードを示します。 CRXリポジトリ内のXDPのパスをこのサーブレットに渡します。

次に、 com.aemformssamples.documentservices.core.DocumentServicesのrenderAndExtendXdpメソッドを呼び出します。

Reader用の拡張PDFは、呼び出し元のアプリケーションにストリーミングされます

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

ローカルサーバーでテストするには、次の手順に従います
1. [DevelopingWithServiceUserバンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [AEM FormsDocumentServicesバンドルをダウンロードしてインストールする](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [パッケージマネージャーを使用して、この記事に関連するアセットをAEMにダウンロードおよび読み込みます](assets/renderandextendxdp.zip)
   * このパッケージには、サンプルポータルとxdpファイルが含まれています
1. 「fd-service」Readerに拡張機能証明書を追加する
1. ブラウザーで[ポータルWebページ](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)を参照します。
1. PDFアイコンをクリックしてxdpをレンダリングし、「Extended」のPDFを取得します。Reader



