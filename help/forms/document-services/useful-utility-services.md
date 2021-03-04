---
title: 役に立つユーティリティサービス
description: AEM Forms開発者向けの便利なユーティリティサービス
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 5%

---


# 役に立つユーティリティサービス

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

サンプルバンドルは、[ここから](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)ダウンロードできます。

## ユーティリティサービスを使用するサンプルコード

次のコードは、JSPページで、文字列からorg.w3c.dom.ドキュメントを作成し、ドキュメントを変換してCRXリポジトリに格納するために使用されたコードです。以下に例を示します。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar)を展開し、バンドルを開始する必要があります。


これらのUtilityサービスを使用してCRXリポジトリにドキュメントを保存する場合は、[サービスユーザー記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms)を使用して開発中の記事に従ってください。 fd-serviceユーザーに、適切なCRXフォルダーの[必要な権限](http://localhost:4502/useradmin)を必ず指定してください。

