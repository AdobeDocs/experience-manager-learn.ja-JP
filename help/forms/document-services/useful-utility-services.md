---
title: 便利なユーティリティサービス
description: AEM Forms開発者向けの便利なユーティリティサービス
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 2%

---

# 便利なユーティリティサービス

このサンプルバンドルは、AEM Forms開発者が使用できる便利なユーティリティサービスを提供します。 以下のサービスを利用できます。


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

サンプルバンドルは、 [ここからダウンロード](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## ユーティリティサービスを使用するサンプルコード

以下は、次のコードスニペットに示すように、JSP ページで、文字列から org.w3c.dom.Document を作成し、ドキュメントを変換して CRX リポジトリに保存するために使用されたコードです。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


をデプロイする必要があります [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) バンドルを起動します。


これらのユーティリティサービスを使用して CRX リポジトリにドキュメントを保存する場合は、 [サービスユーザー記事を使用した開発](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). 必ず [必要な権限](http://localhost:4502/useradmin) を fd-service ユーザーに対する適切な CRX フォルダーに配置します。
