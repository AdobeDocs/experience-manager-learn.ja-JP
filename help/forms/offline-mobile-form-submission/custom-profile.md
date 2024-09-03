---
title: HTML5 フォーム送信時のAEM トリガーワークフロー – カスタムプロファイルの作成
description: カスタムプロファイルを作成し、部分的に入力されたHTML5 フォームのデータを含むインタラクティブ PDF をダウンロードします
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 87%

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
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## インタラクティブ PDF を生成

次に、インタラクティブ PDF をレンダリングして呼び出し元のアプリケーションに PDF を戻すサーブレットコードを示します。サーブレットは、カスタム DocumentServices OSGi サービスの `mobileFormToInteractivePdf` メソッドを呼び出します。

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### インタラクティブ PDF のレンダリング

次のコードは、[Forms Service API](https://helpx.adobe.com/jp/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) を利用して、モバイルフォームからのデータを含むインタラクティブ PDF をレンダリングします。

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## 次の手順

[フォーム送信の処理](./handle-form-submission.md)
