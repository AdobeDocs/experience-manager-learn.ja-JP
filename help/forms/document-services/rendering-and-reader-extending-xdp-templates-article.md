---
title: 使用権限を持つ PDF への XDP のレンダリング
description: PDF への使用権限の適用
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
duration: 150
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 100%

---

# 使用権限を持つ PDF への XDP のレンダリング{#rendering-xdp-into-pdf-with-usage-rights}

一般的なユースケースは、XDP を PDF にレンダリングし、レンダリングされた PDF に Reader Extensions を適用することです。

例えば、AEM Forms のフォームポータルで、ユーザーが XDP をクリックすると、XDP を PDF としてレンダリングし、Reader が PDF を拡張できます。


このユースケースを実現するには、次の手順を実行する必要があります。

* Reader Extensions 証明書を「fd-service」ユーザーに追加します。Reader Extensions 証明書を追加する手順については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=ja)を参照してください


* また、[Reader Extensions 資格情報の設定](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=ja)については、ビデオを参照してください。


* レンダリングして使用権限を適用するカスタム OSGi サービスを作成します。これを行うコードを以下に示します。

## XDP のレンダリングおよび使用権限の適用 {#render-xdp-and-apply-usage-rights}

* 行 7：FormsService の renderPDFForm を使用して、XDP から PDF を生成します。

* 8～14 行：適切な使用権限が設定されます。これらの使用権限は OSGi 設定から取得されます。

* 行 20：サービスユーザー fd-service に関連付けられた resourceresolver を使用します

* 行 24：使用権限の適用には、DocumentAssuranceService の SecureDocument メソッドが使用されます

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

次のスクリーンショットは、公開された設定プロパティを示しています。一般的な使用権限のほとんどは、この設定を通じて公開されます。

![設定プロパティ](assets/configurationproperties.gif)

次のコードは、OSGi 設定の作成に使用するコードを示しています

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

## サーブレットを作成して PDF をストリーミングします {#create-servlet-to-stream-the-pdf}

次の手順では、GET メソッドを使用してサーブレットを作成し、Reader 用の拡張 PDF をユーザーに返します。この場合、ユーザーはファイルシステムに PDF を保存するように求められます。 これは PDF が動的 PDF としてレンダリングされ、ブラウザーに付属している PDF ビューアが動的 PDF を処理しないためです。

次にサーブレットのコードを示します。CRX リポジトリ内の XDP のパスをこのサーブレットに渡します。

次に、com.aemformssamples.documentservices.core.DocumentServices の renderAndExtendXdp メソッドを呼び出します。

その後、Reader 用の拡張 PDF は、呼び出し元のアプリケーションにストリーミングされます

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

ローカルサーバーでこれをテストするには、次の手順に従ってください
1. [DevelopingWithServiceUser バンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [AEMFormsDocumentServices バンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [カスタムポータルテンプレートの html をダウンロードします](assets/render-and-extend-template.zip)
1. [パッケージマネージャーを使用して、この記事に関連するアセットをダウンロードし、AEM に読み込みます](assets/renderandextendxdp.zip)
   * このパッケージには、サンプルポータルと XDP ファイルが含まれています
1. Reader Extensions 証明書を「fd-service」ユーザーに追加します
1. ブラウザーで[ポータル web ページ](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)を表示します
1. PDF アイコンをクリックして、使用権限が適用された PDF ファイルとして XDP をレンダリングします。
