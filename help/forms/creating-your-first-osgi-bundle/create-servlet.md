---
title: AEM Formsでの最初のサーブレットの作成
description: 最初の Sling サーブレットを構築し、データをフォームテンプレートと結合します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 3%

---

# Sling Servlet

サーブレットは、要求応答プログラミングモデルを介してアクセスするアプリケーションをホストするサーバーの機能を拡張するために使用されるクラスです。 このようなアプリケーションの場合、サーブレットテクノロジーは HTTP 固有のサーブレットクラスを定義します。
すべてのサーブレットは、ライフサイクルメソッドを定義するサーブレットインターフェイスを実装する必要があります。


AEMのサーブレットは、OSGi サービスとして登録できます。読み取り専用実装または SlingAllMethodsServlet 用に SlingSafeMethodsServlet を拡張して、すべての RESTful 操作を実装できます。

## サーブレットコード

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

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

* 開く **コマンドプロンプトウィンドウ**
* `c:\aemformsbundles\mysite\core` に移動します。
* コマンドを実行します。 `mvn clean install -PautoInstallBundle`
* 上記のコマンドは、localhost:4502 上で実行されているAEMインスタンスにバンドルを自動的にビルドおよびデプロイします。

バンドルは次の場所でも利用できます。 `C:\AEMFormsBundles\mysite\core\target`. バンドルは、 [Felix Web コンソール。](http://localhost:4502/system/console/bundles)


## サーブレットリゾルバーのテスト

ブラウザーで [サーブレットリゾルバー URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). これにより、以下のスクリーンショットに示すように、特定のパスに対して呼び出されたサーブレットが表示されます
![servlet-resolver](assets/servlet-resolver.JPG)

## Postmanを使用したサーブレットのテスト

![Postmanを使用したサーブレットのテスト](assets/test-servlet-postman.JPG)
