---
title: JSON スキーマとデータを使用したAEM Forms[Part2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: マルチパートチュートリアルで、JSON スキーマを使用したアダプティブフォームの作成と、送信されたデータのクエリに関する手順を説明します。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 送信したデータをデータベースに保存中


>[!NOTE]
>
>MySQL 8 は JSON データ型をサポートしているので、データベースとして使用することをお勧めします。 また、MySQL DB 用の適切なドライバをインストールする必要があります。 この場所https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12にあるドライバを使用しました。

送信されたデータをデータベースに格納するには、連結されたデータとフォーム名、および保存を抽出するサーブレットを作成します。 フォームの送信を処理し、afBoundData をデータベースに保存するための完全なコードを以下に示します。

フォームの送信を処理するためのカスタム送信を作成しました。 このカスタム送信の post.submit.jsp では、POSTをサーブレットに転送します。

カスタム送信の詳細については、こちらを参照してください [記事](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

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

* [zip ファイルをダウンロードして展開します。](assets/aemformswithjson.zip)
* JSON スキーマを使用してアダプティブフォームを作成します。 この記事のアセットの一部として提供されている JSON スキーマを使用できます。 フォームの送信アクションが適切に設定されていることを確認します。 送信アクションは、「CustomSubmitHelpx」に設定する必要があります。
* MySQL Workbench ツールを使用して schema.sql ファイルをインポートし、MySQL インスタンスでスキーマを作成します。 schema.sql ファイルは、このチュートリアルアセットの一部としても提供されています。
* Felix Web コンソールから Apache Sling 接続プールに入れられたデータソースを設定する
* データソース名に「aemformswithjson」という名前を付けてください。 これは、指定されたサンプル OSGi バンドルで使用される名前です
* プロパティについては、上記の画像を参照してください。 これは、MySQL をデータベースとして使用することを前提としています。
* この記事のアセットの一部として提供されている OSGi バンドルをデプロイします。
* フォームをプレビューして送信します。
* JSON データは、「schema.sql」ファイルのインポート時に作成されたデータベースに保存されます。
