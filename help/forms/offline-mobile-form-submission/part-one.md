---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---


# カスタムプロファイルの作成

この部分では、[カスタムプロファイルを作成します。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) プロファイルは、XDPをHTMLとしてレンダリングする必要があります。XDPをHTMLとしてレンダリングするためのデフォルトのプロファイルがデフォルトで提供されます。 これは、MobileFormsレンディションサービスのカスタマイズされたバージョンを表します。 Mobile Form Renditionサービスを使用すると、Mobile Serverの外観、動作、およびやりとりをカスタマイズできます。 アドビのカスタムプロファイルでは、guidebridge APIを使用してモバイルフォームに入力されたデータを取得します。 次に、このデータをカスタムサーブレットに送信し、そのサーブレットがインタラクティブPDFを生成し、呼び出し元のアプリケーションにストリーミングします。

`formBridge` JavaScript APIを使用してフォームデータを取得します。 `getDataXML()`メソッドを使用します。

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

成功ハンドラーメソッドでは、AEMで実行しているカスタムサーブレットを呼び出します。 このサーブレットは、モバイルフォームのデータを含むインタラクティブpdfをレンダリングして返します。

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Generate Interactive PDF

次に、インタラクティブpdfをレンダリングし、pdfを呼び出し元のアプリケーションに返すサーブレットコードを示します。 サーブレットは、カスタムDocumentServices OSGiサービスの`mobileFormToInteractivePdf`メソッドを呼び出します。

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
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
        // TODO Add proper error logging
      }
    }
}
```

### Render Interactive PDF

次のコードは、[FormsサービスAPI](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)を使用して、モバイルフォームのデータを使用してインタラクティブPDFをレンダリングします。

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

部分的に完成したモバイルフォームからインタラクティブPDFをダウンロードする機能を表示するには、[ここをクリック](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content)してください。
PDFをダウンロードしたら、次の手順は、PDFをAEMワークフローのトリガーに送信することです。 このワークフローは、送信されたPDFのデータを結合し、レビュー用の非インタラクティブPDFを生成します。

この使用例で作成したカスタムプロファイルは、このチュートリアルのアセットの一部として使用できます。
