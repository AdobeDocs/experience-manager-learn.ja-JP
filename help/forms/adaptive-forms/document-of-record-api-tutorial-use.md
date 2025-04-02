---
title: API を使用して AEM Formsでレコードのドキュメントを生成する
description: レコードのドキュメント（DOR）をプログラムで生成する
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '239'
ht-degree: 100%

---

# API を使用して AEM Forms でレコードのドキュメントを生成する {#using-api-to-generate-document-of-record-with-aem-forms}

レコードのドキュメント（DOR）をプログラムで生成する

この記事では、`com.adobe.aemds.guide.addon.dor.DoRService API` を使用して **レコードのドキュメント**&#x200B;をプログラムで生成する方法を説明します。[レコードのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html?lang=ja)は、アダプティブフォームで取り込まれる PDF バージョンのデータです。

1. コードスニペットを次に示します。 最初の行は DOR サービスを取得します。
1. DoROptions を設定します。
1. DoRService の render メソッドを呼び出し、DoROptions オブジェクトを render メソッドに渡します。

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

1. [パッケージマネージャーを使用して記事アセットをダウンロードしてインストールします](assets/dor-with-api.zip)
1. 「[サービスユーザーを作成する](service-user-tutorial-develop.md)」記事の一部として提供されている DevelopingWithServiceUser バンドルをインストールして開始していることを確認してください。
1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper Service を探します。
1. サービスマッピングセクションで、エントリ _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ を必ず入力してください。
1. [フォームを開きます](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. フォームに入力し、「PDF を表示」をクリックします
1. ブラウザーの新しいタブに DOR が表示されます


**トラブルシューティングのヒント**

PDF は、新しいブラウザータブに表示されません。

1. ブラウザーでポップアップをブロックしていないことを確認します
1. 管理者として AEM サーバーを起動していることを確認してください（少なくとも Windows の場合）
1. 「DevelopingWithServiceUser」バンドルが&#x200B;*アクティブ状態*&#x200B;にあることを確認します
1. [システムユーザー](http://localhost:4502/useradmin)「fd-service」が次のノードに対する読み取り、変更、作成の権限を持っていることを確認してください`/content/usergenerated/content/aemformsenablement`
