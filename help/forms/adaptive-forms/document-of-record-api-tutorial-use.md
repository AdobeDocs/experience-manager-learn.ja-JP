---
title: APIを使用したAEM Formsとのレコードのドキュメントの生成
seo-title: APIを使用したAEM Formsとのレコードのドキュメントの生成
description: レコードのドキュメント(DOR)をプログラムで生成する
seo-description: APIを使用したAEM Formsとのレコードのドキュメントの生成
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 4%

---


# APIを使用したAEM Formsでのレコードのドキュメントの生成 {#using-api-to-generate-document-of-record-with-aem-forms}

レコードのドキュメント(DOR)をプログラムで生成する

次の記事は、を使用して、レコードの `com.adobe.aemds.guide.addon.dor.DoRService API` ドキュメントをプログラムで生成する方法を説明します **** 。 [レコードのドキュメント](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) は、アダプティブフォームで取り込まれたデータのPDF版です。

1. コードスニペットを次に示します。 最初の行には、DORサービスが表示されます。
1. DoROptionsを設定します。
1. DoRServiceのrenderメソッドを呼び出し、DoROptionsオブジェクトをrenderメソッドに渡します

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

ローカルシステムで試すには、次の手順に従ってください

1. [Package Managerを使用して記事アセットをダウンロードし、インストールします](assets/dor-with-api.zip)
1. 「サービスユーザーを [作成」記事の一部として提供されたDevelopingWithServiceUserバンドルがインストールされ、開始されていることを確認します](service-user-tutorial-develop.md)
1. [configMgrにログイン](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper Service   を探します。
1. 「サービスのマッピング」セクションで、次のエントリ _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ を確認します。
1. [フォームを開く](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. フォームに入力し、「表示PDF」をクリックします。
1. ブラウザーの新しいタブにDORが表示されます


**トラブルシューティングのヒント**

PDFは新しいブラウザータブに表示されません：

1. ブラウザーでポップアップをブロックしていないことを確認します
1. この [記事で説明されている手順に従っていること](service-user-tutorial-develop.md)
1. &#39;DevelopingWithServiceUser&#39;バンドルがアク *ティブ状態であることを確認してください*
1. システムユーザー&#39;データ&#39;に、次のノードで読み取り、変更、および作成の権限があることを確認してください `/content/usergenerated/content/aemformsenablement`

