---
title: API を使用してAEM Formsでレコードのドキュメントを生成する
description: レコードのドキュメント (DOR) をプログラムで生成する
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 5%

---

# API を使用してAEM Formsでレコードのドキュメントを生成する {#using-api-to-generate-document-of-record-with-aem-forms}

レコードのドキュメント (DOR) をプログラムで生成する

この記事では、 `com.adobe.aemds.guide.addon.dor.DoRService API` 生成する **レコードのドキュメント** プログラムで [レコードのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) は、アダプティブフォームで取り込まれるPDFバージョンのデータです。

1. コードスニペットを次に示します。 最初の行は DOR サービスを取得します。
1. DoROptions を設定します。
1. DoRService の render メソッドを呼び出し、 DoROptions オブジェクトを render メソッドに渡します。

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
1. ここで説明する手順に従っているかを確認します。 [記事](service-user-tutorial-develop.md)
1. 「DevelopingWithServiceUser」バンドルがにあることを確認します。 *活動状態*
1. システムユーザー「 data 」に次のノードの読み取り、変更、作成権限があることを確認します `/content/usergenerated/content/aemformsenablement`
