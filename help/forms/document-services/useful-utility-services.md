---
title: 便利なユーティリティサービス
description: AEM Forms開発者向けの便利なユーティリティサービス
feature: アダプティブフォーム
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 4%

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

サンプルバンドルは、[ここから](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)ダウンロードできます。

## ユーティリティサービスを使用するサンプルコード

以下は、次のコードスニペットに示すように、JSPページで、文字列からorg.w3c.dom.Documentを作成し、ドキュメントを変換してCRXリポジトリに保存するために使用されたコードです。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar)をデプロイし、バンドルを開始する必要があります。


これらのユーティリティサービスを使用してCRXリポジトリにドキュメントを保存する場合は、[サービスユーザーの記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms)を使用した開発に従ってください。 fd-serviceユーザーに、適切なCRXフォルダーの[必要な権限](http://localhost:4502/useradmin)を必ず指定してください。

