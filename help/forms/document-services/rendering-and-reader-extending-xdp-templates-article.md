---
title: 使用権限を持つPDFへの XDP のレンダリング
description: PDF に使用権限を適用
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# 使用権限を持つPDFへの XDP のレンダリング{#rendering-xdp-into-pdf-with-usage-rights}

一般的な使用例は、xdp をPDFにレンダリングし、レンダリングPDFにReader拡張を適用することです。

例えば、AEM Formsのフォームポータルで、ユーザーが XDP をクリックすると、XDP をPDFとしてレンダリングし、Reader がPDFを拡張できます。


この使用例を達成するには、次の手順を実行する必要があります。

* Reader拡張証明書を「fd-service」ユーザーに追加します。 Extensions Extensions 証明書を追加する手順がReaderされます。 [ここ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=ja)


* また、 [設定，Reader拡張資格情報](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* 使用権限をレンダリングして適用するカスタム OSGi サービスを作成します。 これをおこなうコードを次に示します

## Render XDP と Apply usage rights {#render-xdp-and-apply-usage-rights}

* 行 7:FormsService の renderPDFForm を使用して、XDP からPDFを生成します。

* 8～14 行目：適切な使用権限が設定されます。 これらの使用権限は OSGi 設定から取得されます。

* 行 20 :サービスユーザー fd-service に関連付けられた resourceresolver を使用します。

* 行 24:使用権限の適用には、DocumentAssuranceService の secureDocument メソッドが使用されます

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

![設定プロパティ](assets/configurationproperties.gif)

次のコードは、OSGi 設定の構築に使用するコードを示しています

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

## サーブレットを作成してPDFをストリーミング {#create-servlet-to-stream-the-pdf}

次の手順では、GETメソッドを使用してサーブレットを作成し、読み取り用拡張PDFをユーザーに返します。 この場合、ユーザーは、ファイル・システムにPDFを保存するように求められます。 これは、PDFがダイナミックPDFとしてレンダリングされ、ブラウザーに付属の pdf ビューアではダイナミック pdf が処理されないためです。

次に、サーブレットのコードを示します。 CRX リポジトリ内の XDP のパスをこのサーブレットに渡します。

次に、com.aemformssamples.documentservices.core.DocumentServices の renderAndExtendXdp メソッドを呼び出します。

その後、リーダ拡張PDFは、呼び出し元のアプリケーションにストリーミングされます

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
1. [DevelopingWithServiceUser バンドルのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [AEM FormsDocumentServices バンドルをダウンロードしてインストールする](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [カスタムポータルテンプレートの html をダウンロード](assets/render-and-extend-template.zip)
1. [パッケージマネージャーを使用して、この記事に関連するアセットをダウンロードし、AEMに読み込みます](assets/renderandextendxdp.zip)
   * このパッケージには、サンプルポータルと xdp ファイルが含まれています
1. Reader拡張証明書を&quot;fd-service&quot;ユーザーに追加
1. ブラウザーで次の場所を指定します。 [ポータル Web ページ](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 使用権限が適用された pdf ファイルとして xdp をレンダリングするには、pdf アイコンをクリックします。
