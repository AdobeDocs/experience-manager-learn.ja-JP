---
title: サーブレットの作成
description: サーブレットを作成して、フォームデータを保存する POST 要求を処理します
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6539
thumbnail: 6539.pg
topic: Development
role: Developer
level: Experienced
exl-id: a24ea445-3997-4324-99c4-926b17c8d2ac
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '88'
ht-degree: 100%

---

# サーブレットの作成

次に、カスタム OSGi サービスの適切なメソッドを呼び出すサーブレットを作成します。 このサーブレットはアダプティブフォームのデータ、添付ファイル情報にアクセスでき、一部が完了したアダプティブフォームを取得するために使用できる一意のアプリケーション ID を返します。

このサーブレットは、ユーザーがアダプティブフォームの「保存して終了」ボタンをクリックすると呼び出されます。

```java
package saveandresume.core.servlets;

import java.io.PrintWriter;

import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
  private Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
  private static final long serialVersionUID = 1 L;
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;

  public void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    final String afData = request.getParameter("data");
    final String tel = request.getParameter("mobileNumber");
    log.debug("$$$The telephone number is  " + tel);
    log.debug("The request parameter data is  " + afData);
    try {
      JsonObject fileMap = JsonParser.parseString(request.getParameter("fileMap")).getAsJsonObject();
      log.debug("The file map is: " + fileMap.toString());
      String newFileMap = saveAndFetchFromDB.storeAFAttachments(fileMap, request);
      String application_id = saveAndFetchFromDB.storeFormData(afData, newFileMap, tel);
      log.debug("The application id:  " + application_id);
      JsonObject jsonObject = new JsonObject();
      jsonObject.addProperty("applicationID", application_id);
      response.setContentType("application/json");
      response.setHeader("Cache-Control", "nocache");
      response.setCharacterEncoding("utf-8");
      PrintWriter out = null;
      out = response.getWriter();
      out.println(jsonObject.toString());
    } catch (Exception ex) {
      log.error(ex.getMessage());
    }
  }

}
```

## 次の手順

[保存されたフォームデータを持つフォームのレンダリング](./retrieve-saved-form.md)
