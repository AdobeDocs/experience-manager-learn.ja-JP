---
title: 「invoke DDX」操作を使用したPDFファイルのアセンブリ
description: 必要なPOSTーを指定して、DDX エンドポイントを呼び出すパラメーターリクエストを作成します
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 16%

---

# POST呼び出し


次の手順では、必要なパラメーターを使用して、エンドポイントに対する HTTPPOST呼び出しをおこないます。 DDX ファイルと pdf ファイルは、リソースファイルとして提供されます。 エンドポイントにはトークンベースの認証があり、リクエストヘッダーでアクセストークンを渡します。
Assembler サービスを利用するときは、必要な出力を記述するために Document Description XML（DDX）と呼ばれる XML ベースの言語を使用します。DDX は、PDFがドキュメントの構築ブロックを表す宣言的なマークアップ言語です。次の DDX が、要素ソース要素で識別された 2 つの pdf ドキュメントのマージに使用されました。

```xml
<DDX xmlns="http://ns.adobe.com/DDX/1.0/">
<PDF result="doc3.pdf"> 
	<PDF source="CA-Drivers-Handbook.pdf"/>
 	<PDF source="CA-Parent-Teen-Handbook.pdf"/>
  </PDF>
</DDX>
```

次のコードは、pdf ファイルの組み合わせに使用されました

```java
package com.aemformscs.documentservices;

import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

import org.apache.commons.io.IOUtils;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;

public class AssemblePDFFiles {
  public String SAVE_LOCATION = "c:\\assembledpdf";

  public void assemblePDF(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL ddxFile = classLoader.getResource("ddxfiles/assemble2pdfs.ddx");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File ddx = new File(ddxFile.getPath());
    builder.addBinaryBody("ddx", ddx, ContentType.create("application/xml"), ddx.getName());
    URL url = classLoader.getResource("pdffiles");
    System.out.println(url.getPath());
    File files[] = new File(url.getPath()).listFiles();

    for (int i = 0; i < files.length; i++) {
      System.out.println("Added  " + files[i].getName());
      builder.addBinaryBody(files[i].getName(), files[i], ContentType.APPLICATION_OCTET_STREAM, files[i].getName());
    }
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      System.out.println("The success code is " + response.getStatusLine().getStatusCode());
      InputStream generatedPDF = response.getEntity().getContent();
      byte[] bytes = IOUtils.toByteArray(generatedPDF);
      File saveLocation = new File(SAVE_LOCATION);
      if (!saveLocation.exists()) {
        saveLocation.mkdirs();
      }
      File outputFile = new File(SAVE_LOCATION + File.separator + "assembledPDF.pdf");
      FileOutputStream outputStream = new FileOutputStream(outputFile);
      outputStream.write(bytes);
      outputStream.close();
      System.out.println("AssembledPDF.pdf saved to " + SAVE_LOCATION);

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```
