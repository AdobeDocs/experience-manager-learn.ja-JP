---
title: API を使用してAEM Formsでレコードのドキュメントを生成する
description: レコードのドキュメント (DOR) をプログラムで生成する
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 6%

---

# API を使用してAEM Formsでレコードのドキュメントを生成する {#using-api-to-generate-document-of-record-with-aem-forms}

レコードのドキュメント (DOR) をプログラムで生成する

この記事では、 `com.adobe.aemds.guide.addon.dor.DoRService API` 生成する **レコードのドキュメント** プログラムで [レコードのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) は、アダプティブフォームで取り込まれるPDFバージョンのデータです。

1. コードスニペットを次に示します。 最初の行は DOR サービスを取得します。
1. DoROptions を設定します。
1. DoRService の render メソッドを呼び出し、 DoROptions オブジェクトを render メソッドに渡します。

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

ローカルシステムでこれを試すには、次の手順に従ってください

1. [パッケージマネージャーを使用して記事アセットをダウンロードしてインストールする](assets/dor-with-api.zip)
1. の一部として提供された DevelopingWithServiceUser バンドルをインストールし、開始したことを確認します。 [サービスユーザーの作成記事](service-user-tutorial-develop.md)
1. [configMgr にログイン](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper Service   を探します。
1. 次のエントリを必ず入力してください _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ （「 Service Mappings 」セクション）
1. [フォームを開く](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. フォームに入力し、「表示PDF」をクリックします
1. ブラウザーの新しいタブに DOR が表示されます


**トラブルシューティングのヒント**

PDFは、新しいブラウザータブに表示されません：

1. ブラウザーでポップアップをブロックしていないことを確認します
1. 管理者としてAEMサーバーを起動していることを確認してください（少なくとも Windows の場合）
1. 「DevelopingWithServiceUser」バンドルがにあることを確認します。 *活動状態*
1. [システムユーザーを確認します。](http://localhost:4502/useradmin) &#39;fd-service&#39;には、次のノードに対する読み取り、変更、作成の権限があります `/content/usergenerated/content/aemformsenablement`
