---
title: HTM5 フォーム送信で AEM ワークフローをトリガーする - カスタムプロファイルの作成
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 100%

---

# カスタムプロファイルを作成

ここでは、[ カスタムプロファイルを作成します。](https://helpx.adobe.com/jp/livecycle/help/mobile-forms/creating-profile.html)プロファイルは、XDP をHTMLとしてレンダリングします。XDP を HTML としてレンダリングするためのデフォルトのプロファイルが初期で提供されます。それはモバイルフォームレンダリングサービスのカスタマイズされたバージョンを表します。モバイルフォームレンダリングサービスを使用して、モバイルフォームの外観、動作、インタラクションをカスタマイズできます。カスタムプロファイルでは、guidebridge API を使用してモバイルフォームに入力されたデータをキャプチャします。次に、このデータがカスタムサーブレットに送信され、このサーブレットがインタラクティブ PDF を生成し、呼び出し元のアプリケーションにストリーミングします。

`formBridge` JavaScript API を使用してフォームデータを取得します。`getDataXML()` メソッドを利用します。

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

成功ハンドラーメソッドでは、AEM で実行されているカスタムサーブレットを呼び出します。このサーブレットは、モバイルフォームのデータを含むインタラクティブ PDF をレンダリングして返します

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

## インタラクティブ PDF を生成

次に、インタラクティブ PDF をレンダリングして呼び出し元のアプリケーションに PDF を戻すサーブレットコードを示します。サーブレットは、カスタム DocumentServices OSGi サービスの `mobileFormToInteractivePdf` メソッドを呼び出します。

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

### インタラクティブ PDF のレンダリング

次のコードは、[Forms Service API](https://helpx.adobe.com/jp/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) を利用して、モバイルフォームからのデータを含むインタラクティブ PDF をレンダリングします。

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

部分的に入力されたモバイルフォームからインタラクティブ PDF をダウンロードする機能を確認するには、[ここをクリックしてください](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content)。
PDF をダウンロードしたら、次の手順では、PDF を AEM ワークフローのトリガーに送信します。このワークフローは、送信された PDF のデータを結合し、レビュー用の非インタラクティブ PDF を生成します。

この使用例で作成したカスタムプロファイルは、このチュートリアルアセットの一部として利用できます。
