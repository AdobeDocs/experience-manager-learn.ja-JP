---
title: AEM Forms での最初のサーブレットの作成
description: 最初の Sling サーブレットを構築し、データをフォームテンプレートと結合します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '201'
ht-degree: 100%

---

# Sling サーブレット

サーブレットは、アプリケーション（リクエスト／応答プログラミングモデルを介してアクセスされる）をホストするサーバーの機能の拡張に使用されるクラスです。このようなアプリケーションの場合、サーブレットテクノロジーでは HTTP 固有のサーブレットクラスを定義します。
すべてのサーブレットは、ライフサイクルメソッドを定義するサーブレットインターフェイスを実装する必要があります。


AEM のサーブレットは OSGi サービスとして登録できます。読み取り専用実装または SlingAllMethodsServlet 用に SlingSafeMethodsServlet を拡張して、すべての RESTful 操作を実装できます。

## サーブレットコード

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## ビルドとデプロイ

プロジェクトを構築するには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます
* `c:\aemformsbundles\mysite\core` に移動します。
* コマンド `mvn clean install -PautoInstallBundle` を実行する
* 上記のコマンドは、localhost:4502 上で実行されている AEM インスタンスにバンドルを自動的にビルドおよびデプロイします。

バンドルは次の場所 `C:\AEMFormsBundles\mysite\core\target` でも利用できます。バンドルは、[Felix web コンソール](http://localhost:4502/system/console/bundles)を使用して AEM にデプロイすることもできます


## サーブレットリゾルバーのテスト

ブラウザーで[サーブレットリゾルバー URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST) にアクセスします。これにより、以下のスクリーンショットに示すように、特定のパスに対して呼び出されたサーブレットが表示されます
![servlet-resolver](assets/servlet-resolver.JPG)

## Postman を使用したサーブレットのテスト

![Postman を使用したサーブレットのテスト](assets/test-servlet-postman.JPG)

## 次の手順

[サードパーティ JAR の組み込み](./include-third-party-jars.md)

