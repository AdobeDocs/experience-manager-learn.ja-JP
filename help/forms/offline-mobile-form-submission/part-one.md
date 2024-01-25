---
title: HTM5 フォーム送信でのトリガーAEMワークフロー — カスタムプロファイルの作成
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 125
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 5%

---

# カスタムプロファイルを作成

ここでは、 [カスタムプロファイル。](https://helpx.adobe.com/jp/livecycle/help/mobile-forms/creating-profile.html) プロファイルは、XDP をHTMLとしてレンダリングします。 デフォルトのプロファイルは、XDP のをHTMLとしてレンダリングするために初期設定で提供されます。 これは、Mobile Forms Rendition サービスのカスタマイズバージョンを表します。 Mobile Forms Rendition サービスを使用して、Mobile Formsの外観、動作およびインタラクションをカスタマイズできます。 カスタムプロファイルでは、guidebridge API を使用してモバイルフォームに入力されたデータをキャプチャします。 次に、このデータがカスタムサーブレットに送信され、このサーブレットがインタラクティブPDFを生成し、呼び出し元のアプリケーションにストリーミングします。

を使用してフォームデータを取得する `formBridge` JavaScript API。 我々は、を利用する `getDataXML()` メソッド：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

成功ハンドラーメソッドでは、AEMで実行されているカスタムサーブレットを呼び出します。 このサーブレットは、モバイルフォームのデータを含むインタラクティブ pdf をレンダリングして返します

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

## インタラクティブPDF

次に、インタラクティブ pdf のレンダリングと呼び出し元のアプリケーションへの pdf の返送を担当するサーブレットコードを示します。 サーブレットがを呼び出します `mobileFormToInteractivePdf` カスタム DocumentServices OSGi サービスのメソッド。

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

### インタラクティブのレンダリングPDF

次のコードは、 [Forms Service API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) モバイルフォームのPDFを使用してインタラクティブデータをレンダリングする場合。

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

部分的に入力されたモバイルフォームからインタラクティブPDFをダウンロードする機能を表示するには、 [ここをクリックしてください](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
PDFをダウンロードしたら、次の手順では、PDFをAEMワークフローのトリガーに送信します。 このワークフローは、送信されたPDFのデータを結合し、レビュー用の非インタラクティブPDFを生成します。

この使用例で作成したカスタムプロファイルは、このチュートリアルアセットの一部として利用できます。
