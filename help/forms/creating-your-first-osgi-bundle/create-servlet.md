---
title: AEM Formsでの最初のサーブレットの作成
description: 最初のSlingサーブレットを構築し、データをフォームテンプレートとマージします。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: c74c6f5627e69e32bbf0098d6b6bab122cace798
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# Sling Servlet

サーブレットは、要求応答プログラミングモデルを介してアクセスするアプリケーションをホストするサーバーの機能を拡張するために使用されるクラスです。 このようなアプリケーションの場合、サーブレットテクノロジーはHTTP固有のサーブレットクラスを定義します。
すべてのサーブレットは、ライフサイクルメソッドを定義するサーブレットインターフェイスを実装する必要があります。


AEM内のサーブレットは、OSGiサービスとして登録できます。読み取り専用実装またはSlingAllMethodsServlet用にSlingSafeMethodsServletを拡張して、すべてのRESTful操作を実装できます。

## サーブレットコード

```java
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

プロジェクトをビルドするには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます。
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンド`mvn clean install -PautoInstallBundle`を実行します。
* 上記のコマンドは、localhost:4502で実行されているAEMインスタンスにバンドルを自動的にビルドしてデプロイします。

バンドルは、次の場所`C:\AEMFormsBundles\learningaemforms\core\target`でも使用できます。 バンドルは、[Felix Webコンソールを使用してAEMにデプロイすることもできます。](http://localhost:4502/system/console/bundles)


## サーブレットリゾルバーのテスト

ブラウザーで[サーブレットリゾルバーURL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST)を参照します。 これにより、以下のスクリーンショットに示すように、特定のパスに対して呼び出されるサーブレットが示されます
![servlet-resolver](assets/servlet-resolver.JPG)

## Postmanを使用したサーブレットのテスト

![test-servlet-postman](assets/test-servlet-postman.JPG)
