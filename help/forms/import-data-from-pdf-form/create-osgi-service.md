---
title: PDF フォームからデータを書き出す OSGi サービスの作成
description: FormsService API を使用した PDFフォームからのデータの書き出し
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: c3032669-154c-4565-af6e-32d94e975e37
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '135'
ht-degree: 100%

---

# データを書き出し

PDF ファイルからアダプティブフォームに入力する最初の手順では、指定した PDF ファイルからデータを書き出し、AEM リポジトリに保存します。

次のコードは、アップロードされた PDF とメッセージからデータを抽出して、アダプティブフォームの入力に使用できる正しい形式を取得するために書き込まれました

```java
public String getFormData(Document pdfForm) {
   DocumentBuilderFactory factory = null;
   DocumentBuilder builder = null;
   org.w3c.dom.Document xmlDocument = null;

   try {
      Document xmlData = formsService.exportData(pdfForm, DataFormat.Auto);
      xmlData.copyToFile(new File("xmlData.xml"));
      factory = DocumentBuilderFactory.newInstance();
      factory.setNamespaceAware(true);
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlData.getInputStream());

      org.w3c.dom.Node xdpNode = xmlDocument.getDocumentElement();
      log.debug("Got xdp " + xdpNode.getNodeName());
      org.w3c.dom.Node datasets = getChildByTagName(xdpNode, "xfa:datasets");
      log.debug("Got datasets " + datasets.getNodeName());
      org.w3c.dom.Node data = getChildByTagName(datasets, "xfa:data");
      log.debug("Got data " + data.getNodeName());
      org.w3c.dom.Node topmostSubform = getChildByTagName(data, "topmostSubform");

      if (topmostSubform != null) {

         org.w3c.dom.Document newXmlDocument = builder.newDocument();
         org.w3c.dom.Node importedNode = newXmlDocument.importNode(topmostSubform, true);
         newXmlDocument.appendChild(importedNode);
         Document aemFDXmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXmlDocument);
         aemFDXmlDocument.copyToFile(new File("aemFDXmlDocument.xml"));
         // saveDocumentInCrx is an utility method of the custom DocumentServices service. 
         return documentServices.saveDocumentInCrx("/content/exporteddata", ".xml", aemFDXmlDocument);
      }

   } catch (Exception e) {
      log.debug("Error:  " + e.getMessage());

   }
   return null;
}
```

以下は、適切な名前空間を持つ _**topmostSubForm**_ を抽出するために書き込まれたユーティリティ関数です

```java
private static org.w3c.dom.Node getChildByTagName(org.w3c.dom.Node parent, String tagName) {
   NodeList nodeList = parent.getChildNodes();
   for (int i = 0; i < nodeList.getLength(); i++) {
      org.w3c.dom.Node node = nodeList.item(i);
      if (node.getNodeType() == org.w3c.dom.Node.ELEMENT_NODE && node.getNodeName().equals(tagName)) {
         return node;
      }
   }
   return null;
}
```

抽出したデータは、crx リポジトリの /content/exporteddata ノードに保存されます。書き出されたデータのファイルパスは、アダプティブフォームに入力するために呼び出し元のアプリケーションに返されます。

## 次の手順

[PDF ファイルからデータを読み込み](./populate-adaptive-form.md)
