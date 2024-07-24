---
title: 権限パスワードでPDFを暗号化
description: DocAssuranceService を使用したPDFの暗号化
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: b823f9e294c42ba258049a942816f9a154a6e1a6
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 31%

---

# 権限パスワードでPDFを暗号化

PDFドキュメントをコピー、編集、または印刷するには、オーナーパスワードまたはマスターパスワードとも呼ばれるアクセス許可パスワードが必要です。 [DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) API を使用して、プログラムによってPDFに権限パスワードを適用する方法を説明します

次の JSP コードは、権限パスワードでPDFを暗号化します。

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**システム上のサンプルパッケージをテストするには：**

[AEM パッケージマネージャーを使用して、パッケージをダウンロードしてインストールします。](assets/encryptpdf.zip)

**パッケージをインストールしたら、次の URL をAdobe許可リストに加える Granite CSRF フィルターの OSGi 設定に追加します。***

1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)。
1. Adobe Granite CSRF フィルターを検索します。
1. 除外されたセクションに次のパスを追加して、保存します。
1. /content/AemFormsSamples/encrypt

## サンプルのテスト

サンプルコードをテストするには、様々な方法があります。Postman アプリを使用するのが最もすばやく簡単です。Postmanでは、サーバーに対してPOSTリクエストを行うことができます。次のスクリーンショットは、POST リクエストが機能するために必要なリクエストパラメーターを示しています。 リクエストを送信する前に、適切な認証タイプを指定するようにしてください。

![encrypt-pdf-postman](assets/encrypt-pdf-postman.png)
