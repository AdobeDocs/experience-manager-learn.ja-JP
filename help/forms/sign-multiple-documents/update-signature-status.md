---
title: データベース内のフォームの署名ステータスの更新
description: AEM ワークフローを使用した、データベース内の署名済みフォームの署名ステータスの更新
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
duration: 42
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 100%

---

# 署名ステータスの更新

ユーザーが署名行為を完了すると、UpdateSignatureStatus ワークフローがトリガーされます。以下は、ワークフローの流れです

![main-workflow](assets/update-signature.PNG)

署名ステータスの更新はカスタムプロセスステップです。
カスタムプロセスステップを実装する主な理由は、AEM ワークフローを拡張することです。以下は、署名ステータスの更新に使用するカスタムコードです。
このカスタムプロセスプロセスのコードは、SignMultipleForms サービスを参照します。


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## アセット

署名ステータスの更新ワークフローは、[こちらからダウンロード](assets/update-signature-status-workflow.zip)できます

## 次の手順

[署名用の次のフォームを表示するための概要ステップのカスタマイズ](./customize-summary-component.md)
