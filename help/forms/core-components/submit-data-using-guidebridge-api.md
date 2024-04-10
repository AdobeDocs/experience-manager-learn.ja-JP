---
title: GuideBridge API を使用したフォームデータへのアクセス
description: コアコンポーネントベースのアダプティブフォーム用に GuideBridge API を使用して、フォームデータと添付ファイルにアクセスします。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
source-git-commit: 73c15a195c438dd7a07142bba719c6f820bf298a
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# GuideBridge API を使用したフォームデータのPOST化

フォームの「保存と再開」を使用すると、ユーザーはフォームの入力の進行状況を保存し、後で再開できます。
次の手順に従って、アダプティブFormsで Geolocation API を使用しました。 このユースケースを実現するには、GuideBridge API を使用してフォームデータにアクセスし、REST エンドポイントに送信して保存および取得する必要があります。

フォームデータは、ルールエディターを使用するボタンのクリックイベントで保存されます
![ルールエディター](assets/rule-editor.png)

指定されたエンドポイントにデータを送信するために、次の JavaScript 関数を作成しました

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST end point
* @param {string} endpoint in Stringformat
* @return {string} 
 */

function submitFormDataAndAttachments(endpoint) {

    guideBridge.getFormDataObject({
        success: function(resultObj) {
            afFormData = resultObj.data.data;
            var formData = new FormData();
            formData.append("dataXml", afFormData);
            for (i = 0; i < resultObj.data.attachments.length; i++) {
                var attachment = resultObj.data.attachments[i];
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            }
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    console.log("successfully saved");
                    var fld = guideBridge.resolveNode("$form.confirmation");
                    return " Form data was saved successfully";

                }
            };
            xhttp.open('POST', endpoint)
            xhttp.send(formData);
        }
    });

}
```



## サーバー側コード

次のサーバー側 Java コードは、フォームデータを処理するために記述されました。 以下は、上記の JavaScript の XHR 呼び出しを介して呼び出される、AEMで動作する Java サーブレットです。

```java
package com.azuredemo.core.servlets;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;
@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1 L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());
   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List < RequestParameter > listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of list is " + listOfRequestParameters.size());
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("is this request parameter a form field?" + requestParameter.isFormField());
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
