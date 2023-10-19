---
title: OSGi サービスを作成してデータを書き出しフォームからPDF
description: FormsService API を使用してPDFフォームからデータを書き出す
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---

# データを書き出し

PDFファイルからアダプティブフォームにデータを入力する最初の手順は、指定されたPDFファイルからデータを書き出し、AEMリポジトリに保存することです。

次のコードは、アップロードされた PDF からデータを抽出し、アダプティブフォームの入力に使用できる以上の正しい形式を取得するためにマッサージされたものです

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

次に、 _**topmostSubForm**_ 適切な名前空間を持つ

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

抽出したデータは、crx リポジトリの/content/exporteddata ノードに保存されます。 書き出されたデータのファイルパスが呼び出し元のアプリケーションに返され、アダプティブフォームに入力されます。

## 次の手順

[PDF ファイルからデータを読み込み](./populate-adaptive-form.md)

