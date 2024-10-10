---
title: DocAssurance API の使用
description: Java の Apache HTTP コンポーネントを使用して DocAssurance API を呼び出すサンプルコード
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-15508
exl-id: 40617082-4d23-4c91-a016-2d947187052b
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: ht
source-wordcount: '276'
ht-degree: 100%

---

# DocAssurance API の使用

[DocAssurance サービス](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance)は、署名、認証、署名フィールドの追加、暗号化、復号化など、PDF ドキュメントに対して様々なデジタル署名または暗号化処理を実行する機能を提供します。
この記事では、API の使用を開始する Java コードスニペットを提供します。コードスニペットはアクセストークンを利用します。[この記事では、アクセストークンの生成に必要な手順を説明します](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction)。


<span class="preview">この機能は、早期導入プログラムで利用できます。早期導入プログラムに参加し、この機能の利用申請をするには、公式のメール ID で aem-forms-ea@adobe.com までメールを送信してください。</span>


## 前提条件

* AEM Forms Cloud Service でのエクスペリエンス
* [Apache HTTP コンポーネント](https://hc.apache.org/httpcomponents-client-4.5.x/)の使用経験
* AEM Forms Cloud Service 環境へのアクセス

## ドキュメントの検査

検査 API を使用して、特定の PDF ドキュメントのセキュリティの種類を取得します。次のコードスニペットは、作業を開始する際に役立ちます。

```java
...
File fileToInspect = new File("path_to_your_pdf_file)";
HttpPost httpPost = new HttpPost("<your_aem_forms_instance>/adobe/forms/document/assure/inspect");
httpPost.addHeader("Authorization", "Bearer " + accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToInspect);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
try
{
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)   
    {
        String json = EntityUtils.toString(response.getEntity(), StandardCharsets.UTF_8);
        log.info("The mode of encryption is  " + JsonParser.parseString(json).getAsJsonObject().get("mode").getAsString());
    }

} 
catch (Exception e)
{
   log.error(e.getMessage());
}
...
```


## ドキュメントの暗号化

暗号化 API を使用して、パスワードで PDF ドキュメントを暗号化します。次のサンプルコードスニペットは、特定の PDF を暗号化します。

```java
...
File fileToEncrypt = new File("path_to_your_pdf_file");
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer " + accessToken ");
MultipartEntityBuilder builder = MultipartEntityBuilder.create(); byte[] fileContent = FileUtils.readFileToByteArray(fileToEncrypt); builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
String config = "{\"mode\":\"ENCRYPT_WITH_PASSWORD\",\"params\":{\"openPassword\":\"adobe\",\"permPassword\":\"systems\",\"permissions\":[\"ALL_PERM\"]}}";
 builder.addTextBody("config", config, ContentType.APPLICATION_JSON);
try
 {
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)
    {
       InputStream generatedPDF = response.getEntity().getContent();
       byte[] bytes = IOUtils.toByteArray(generatedPDF);
       File encryptedFile = new File("c:\\aem_forms_cs_api\\encrypted.pdf");
       FileOutputStream outputStream = new FileOutputStream(encryptedFile);
       outputStream.write(bytes);
       outputStream.close();
    }

}
catch (Exception e)
 {
    log.error(e.getMessage());
 }

...
```

## PDF への署名フィールドの追加

署名フィールド API を使用して、指定された PDF に署名を追加します。次のサンプルコードスニペットでは、ドキュメントの 4 ページ目に SignHere という署名フィールドを追加しています。

```java
...
File pdfFile = new File(pdfFile1.getPath());
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer "+accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(pdfFile);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("field", "SignHere", ContentType.TEXT_PLAIN);
String rectangle = "{\"lowerLeftX\":1,\"lowerLeftY\":40,\"width\":100,\"height\":100}";
builder.addTextBody("rectangle", rectangle, ContentType.APPLICATION_JSON);
```


## 暗号化を削除

暗号化 API に対して PUT 操作を使用し、指定された PDF から暗号化を削除します。開始するには、次の Java コードスニペットが役立ちます。

```java
...
File fileToDecrypt = new File("path_to_your_pdf_file");

HttpPut httpPut = new HttpPut(putURL);
httpPut.addHeader("Authorization", "Bearer " + accessToken);

MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToDecrypt);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("config", "systems", ContentType.TEXT_PLAIN);

try {
    HttpEntity entity = builder.build();
    httpPut.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPut);

if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File encryptionRemoved = new File("c:\\aem_forms_cs_api\\encryption_removed.pdf");
        FileOutputStream outputStream = new FileOutputStream(encryptionRemoved);
        outputStream.write(bytes);
        outputStream.close();
        httpclient.close();
    }
} catch (Exception e) {
    log.error(e.getMessage());
}
...
```

### Postman コレクション

API の Postman コレクションは、[テスト目的でここからダウンロード](assets/DocAssuranceAPI.postman_collection.json)できます。API を呼び出すには、基本認証またはベアラートークンタイプの認証を使用できます。
