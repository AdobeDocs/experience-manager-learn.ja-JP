---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# カスタムプロファイルの作成

ここでは、[カスタムプロファイルを作成します。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) プロファイルは、XDPをHTMLとしてレンダリングします。XDPのをHTMLとしてレンダリングするためのデフォルトのプロファイルが初期設定で提供されます。 これは、Mobile Forms Renditionサービスのカスタマイズバージョンを表します。 Mobile Form Renditionサービスを使用して、Mobile Formsの外観、動作およびインタラクションをカスタマイズできます。 カスタムプロファイルでは、guidebridge APIを使用してモバイルフォームに入力されたデータをキャプチャします。 次に、このデータがカスタムサーブレットに送信され、このサーブレットはインタラクティブPDFを生成し、呼び出し元のアプリケーションにストリーミングします。

`formBridge` JavaScript APIを使用してフォームデータを取得します。 `getDataXML()`メソッドを使用します。

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

成功ハンドラーメソッドでは、AEMで実行されているカスタムサーブレットを呼び出します。 このサーブレットは、モバイルフォームのデータを含むインタラクティブpdfをレンダリングして返します

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

## インタラクティブPDFを生成

次に、インタラクティブpdfのレンダリングと呼び出し元のアプリケーションへのpdfの返送を担当するサーブレットコードを示します。 このサーブレットは、カスタムDocumentServices OSGiサービスの`mobileFormToInteractivePdf`メソッドを呼び出します。

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

### インタラクティブPDFをレンダリング

次のコードは、[Forms Service API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)を使用して、モバイルフォームのデータを使用してインタラクティブPDFをレンダリングします。

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

部分的に完成したモバイルフォームからインタラクティブPDFをダウンロードする機能を確認するには、[ここ](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content)をクリックしてください。
PDFがダウンロードされたら、次の手順はPDFをAEMワークフローにトリガーすることです。 このワークフローは、送信されたPDFのデータを結合し、レビュー用に非インタラクティブPDFを生成します。

この使用例用に作成されたカスタムプロファイルは、このチュートリアルアセットの一部として利用できます。
