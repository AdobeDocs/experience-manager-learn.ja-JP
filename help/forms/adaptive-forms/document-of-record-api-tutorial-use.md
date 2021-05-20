---
title: APIを使用したAEM Formsでのレコードのドキュメントの生成
seo-title: APIを使用したAEM Formsでのレコードのドキュメントの生成
description: レコードのドキュメント(DOR)をプログラムで生成する
seo-description: APIを使用したAEM Formsでのレコードのドキュメントの生成
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 5%

---


# APIを使用してAEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}でレコードのドキュメントを生成する

レコードのドキュメント(DOR)をプログラムで生成する

この記事では、`com.adobe.aemds.guide.addon.dor.DoRService API`を使用して&#x200B;**レコードのドキュメント**&#x200B;をプログラムで生成する方法を説明します。 [Document of Recordは、](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) アダプティブフォームで取り込まれるデータのPDF版です。

1. コードスニペットを次に示します。 最初の行にDORサービスが表示されます。
1. DoROptionsを設定します。
1. DoRServiceのrenderメソッドを呼び出し、 DoROptionsオブジェクトをrenderメソッドに渡します

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

ローカルシステムでこれを試すには、次の手順に従ってください

1. [パッケージマネージャーを使用して記事アセットをダウンロードし、インストールする](assets/dor-with-api.zip)
1. [Create Service User記事](service-user-tutorial-develop.md)の一部として提供されるDevelopingWithServiceUserバンドルをインストールして開始したことを確認します。
1. [configMgrにログインします。](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper Service   を探します。
1. 「サービスマッピング」セクションで、次のエントリ&#x200B;_DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_&#x200B;を確認します。
1. [フォームを開く](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. フォームに入力し、「PDFを表示」をクリックします。
1. ブラウザーの新しいタブにDORが表示されます


**トラブルシューティングのヒント**

PDFが新しいブラウザータブに表示されない：

1. ブラウザーでポップアップをブロックしていないことを確認します。
1. この[記事](service-user-tutorial-develop.md)で説明されている手順に従ったことを確認します。
1. &#39;DevelopingWithServiceUser&#39;バンドルが&#x200B;*アクティブな状態*&#x200B;であることを確認します。
1. システムユーザー「 data 」に次のノード`/content/usergenerated/content/aemformsenablement`に対する読み取り、変更、作成の権限があることを確認します。

