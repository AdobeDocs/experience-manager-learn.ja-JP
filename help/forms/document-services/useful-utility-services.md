---
title: 便利なユーティリティサービス
description: AEM Forms 開発者向けの便利なユーティリティサービス
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '138'
ht-degree: 100%

---

# 便利なユーティリティサービス

このサンプルバンドルは、AEM Forms 開発者が使用できる便利なユーティリティサービスを提供します。次のサービスを利用できます。


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

サンプルバンドルは、[ここからダウンロード](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)できます。

## ユーティリティサービスを使用するサンプルコード

以下は、文字列から org.w3c.dom.Document を作成し、そのドキュメントを変換して CRX リポジトリに保存するために JSP ページで使用されたコードです（次のコードスニペットを参照）。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar?lang=ja) をデプロイし、そのバンドルを起動する必要があります。


これらのユーティリティサービスを利用して CRX リポジトリにドキュメントを保存する場合は、[サービスユーザーを使用した開発](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=ja#adaptive-forms)の記事に従ってください。fd-service ユーザーに適切な CRX フォルダーの[必要な権限](http://localhost:4502/useradmin)を必ず提供してください。
