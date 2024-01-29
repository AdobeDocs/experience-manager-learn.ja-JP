---
title: タスクの割り当て通知のカスタマイズ
description: タスクの割り当て通知メールにフォームデータを含める
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 151
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '438'
ht-degree: 100%

---

# タスクの割り当て通知のカスタマイズ

タスクを割り当てコンポーネントは、タスクをワークフロー参加者に割り当てるために使用します。 タスクがユーザーまたはグループに割り当てられると、定義されたユーザーまたはグループメンバーにメール通知が送信されます。
このメール通知には、通常、タスクに関連する動的データが含まれます。 この動的データは、システム生成された[メタデータプロパティ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html?lang=ja#using-system-generated-metadata-in-an-email-notification)を使用して取得されます 。
送信されたフォームデータの値をメール通知に含めるには、カスタムメタデータプロパティを作成し、メールテンプレートでこれらのカスタムメタデータプロパティを使用する必要があります



## カスタムメタデータプロパティの作成

推奨されるアプローチは、[WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html?lang=ja#getUserMetadataMap--) の getUserMetadata メソッドを実装する OSGI コンポーネントを作成することです。

次のコードは、4 つのメタデータプロパティ（_firstName_、_lastName_、_reason_、 _amountRequested_) を作成し、送信されたデータから値を設定します。 例えば、メタデータプロパティ _firstName_ の値は、送信されたデータから firstName と呼ばれる要素の値に設定されます。 次のコードは、アダプティブフォームの送信済みデータが xml 形式であることを前提としています。 JSON スキーマまたはフォームデータモデルに基づくアダプティブフォームは、JSON 形式のデータを生成します。


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## タスク通知メールテンプレートのカスタムメタデータプロパティを使用する

メールテンプレートでは、次の構文を使用してメタデータプロパティを含めることができます。 ここで amountRequested はメタデータプロパティ `${amountRequested}` です

## カスタムメタデータプロパティを使用するように、タスクを割り当てを設定する

OSGi コンポーネントをビルドして AEM サーバーにデプロイした後、次に示すように、カスタムメタデータプロパティを使用するように、タスクを割り当てコンポーネントを設定します。


![タスク通知](assets/task-notification.PNG)

## カスタムメタデータプロパティの使用を有効にする

![カスタムメタデータプロパティ](assets/custom-meta-data-properties.PNG)

## サーバーで試すには

* [Day CQ メールサービスの設定](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=ja#configuring-the-mail-service)
* 有効なメール ID を[管理者ユーザー](http://localhost:4502/security/users.html)に関連付けます
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、[Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) をダウンロードしてインストールします
* [アダプティブフォーム](assets/request-travel-authorization.zip)をダウンロードして[フォームとドキュメントの UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) から AEM に読み込みます。
* [Web コンソール](http://localhost:4502/system/console/bundles)を使用して、[カスタムバンドル](assets/work-items-user-service-bundle.jar)をデプロイして起動します
* [フォームをプレビューして送信します](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

フォーム送信タスクの割り当て通知が、管理者ユーザーに関連付けられたメール ID に送信されます。 次のスクリーンショットは、タスク割り当て通知の例を示しています

![通知](assets/task-nitification-email.png)

>[!NOTE]
>タスクを割り当て通知用のメールテンプレートは、次の形式にする必要があります。
>
> 件名 = タスクが割り当てられました - `${workitem_title}`
>
> メッセージ = 改行文字を含まないメールテンプレートを表す文字列

## タスクを割り当てメール通知のタスクコメント

場合によっては、前のタスク所有者のコメントを後続のタスク通知に含める必要があります。 タスクの最後のコメントを取り込むためのコードを以下に示します。

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

上記のコードを含むバンドルは、[こちら](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)からダウンロードできます
