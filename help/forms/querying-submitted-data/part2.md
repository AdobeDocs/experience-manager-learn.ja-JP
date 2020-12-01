---
title: JSONスキーマとデータを持つAEM Forms[Part2]
seo-title: JSONスキーマとデータを持つAEM Forms[Part2]
description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
seo-description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# データベースへの送信データの格納


>[!NOTE]
>
>JSONデータ型がサポートされているので、MySQL 8をデータベースとして使用することをお勧めします。 また、MySQL DB用の適切なドライバーをインストールする必要があります。 この場所のドライバを使用しています。 https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

送信されたデータをデータベースに保存するには、サーブレットを作成し、連結されたデータとフォーム名と保存を抽出します。 フォームの送信を処理し、afBoundDataをデータベースに格納するための完全なコードを以下に示します。

フォームの送信を処理するためにカスタム送信を作成しました。 このカスタム送信のpost.POST.jspでは、リクエストをサーブレットに転送します。

カスタム送信プリースの詳細については、この[記事](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)を参照してください。

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

これをシステムで動作させるには、次の手順に従ってください

* [zipファイルをダウンロードして解凍します。](assets/aemformswithjson.zip)
* 「Create AdaptiveForm With JSON」スキーマを参照してください。 この記事アセットの一部として指定されたJSONスキーマを使用できます。 フォームの送信アクションが適切に設定されていることを確認します。 送信アクションは、「CustomSubmitHelpx」に設定する必要があります。
* MySQL Workbenchツールを使用してスキーマ.sqlファイルを読み込み、MySQLインスタンスでスキーマを作成します。 スキーマ.sqlファイルは、このチュートリアルアセットの一部としても提供されます。
* Felix WebコンソールからApache Sling接続プール済みデータソースを設定します
* データソース名を「aemformswithjson」と指定してください。 これは、提供されるサンプルOSGiバンドルで使用される名前です
* プロパティについては、上記の画像を参照してください。 これは、MySQLをデータベースとして使用すると仮定しています。
* この記事アセットの一部として提供されるOSGiバンドルをデプロイします。
* フォームをプレビューし、送信します。
* JSONデータは、「スキーマ.sql」ファイルを読み込んだときに作成されたデータベースに保存されます。
