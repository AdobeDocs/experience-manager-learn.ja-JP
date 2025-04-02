---
title: MySQL Database からのフォームデータの格納と取得 - フォームデータを格納するサーブレット
description: フォームデータの保存と取得に関わる手順について説明する、複数のパートで構成されているチュートリアル
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: dd82f309-dd4e-42ce-8856-e51c898024f5
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '95'
ht-degree: 100%

---

# フォームデータを格納するサーブレット

次の手順では、フォームデータを挿入または更新するサーブレットを作成します。このサーブレットは、OSGi サービスの適切なメソッドを呼び出して、データベースを挿入または更新します。保存されたアダプティブフォームデータは、GUID に関連付けられます。同じ GUID を使用して、フォームデータが更新されます。このサーブレットは、「SaveAndContinueLater」ボタンがクリックされると呼び出されます。

```java
package com.aemforms.saveandcontinue.core.servlets;

import java.io.IOException;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.aemforms.saveandcontinue.core.FetchStoredFormData;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
},
property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdata"
})
public class StoreDataInDB extends SlingAllMethodsServlet {
  private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
  private static final long serialVersionUID = 1L;
  @Reference
  FetchStoredFormData fetchStoredFormData;
  protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    log.debug("Inside my save af data servlet");
    if (request.getParameter("operation").equalsIgnoreCase("update")) {
      log.debug("The operation is update");
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.updateData(request.getParameter("guid"), request.getParameter("formdata"));
      log.debug("The guid I got was  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }
    }

    if (request.getParameter("operation").equalsIgnoreCase("insert")) {
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.storeFormData(request.getParameter("formdata"));
      log.debug("The guid on inserting data  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        log.debug("error in writing response  " + e.getMessage());
      }
    }

  }

}
```
