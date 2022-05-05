---
title: タスクの割り当て通知のカスタマイズ
description: タスクの割り当て通知電子メールにフォームデータを含める
sub-product: forms
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
source-git-commit: eb2a807587ab918be82d00d50bf1b338df58e84c
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 6%

---

# タスクの割り当て通知のカスタマイズ

タスクの割り当てコンポーネントは、タスクをワークフロー参加者に割り当てるために使用します。 タスクがユーザーまたはグループに割り当てられると、定義されたユーザーまたはグループメンバーに電子メール通知が送信されます。
この電子メール通知には、通常、タスクに関連する動的データが含まれます。 この動的データは、生成されたシステムを使用して取得されます [メタデータプロパティ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification).
送信されたフォームデータの値を電子メール通知に含めるには、カスタムメタデータプロパティを作成し、電子メールテンプレートでこれらのカスタムメタデータプロパティを使用する必要があります



## カスタムメタデータプロパティの作成

推奨されるアプローチは、 [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

次のコードは、4 つのメタデータプロパティ (_firstName_,_lastName_,_理由_ および _amountRequested_) をクリックし、送信されたデータから値を設定します。 例えば、メタデータプロパティの場合、 _firstName_&#x200B;の値は、送信されたデータから firstName と呼ばれる要素の値に設定されます。 次のコードは、アダプティブフォームの送信済みデータが xml 形式であることを前提としています。 JSON スキーマまたはフォームデータモデルに基づくアダプティブFormsは、JSON 形式のデータを生成します。


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

## タスク通知電子メールテンプレートのカスタムメタデータプロパティを使用

電子メールテンプレートでは、次の構文を使用して metadata プロパティを含めることができます。 amountRequested は metadata プロパティです。 `${amountRequested}`

## カスタムメタデータプロパティを使用するように Assign Task を設定

OSGi コンポーネントを構築してAEMサーバーにデプロイした後、次に示すように、タスクを割り当てコンポーネントを設定して、カスタムメタデータプロパティを使用します。


![タスク通知](assets/task-notification.PNG)

## カスタムメタデータプロパティの使用の有効化

![Custom Meta Data プロパティ](assets/custom-meta-data-properties.PNG)

## サーバーで試すには

* [Day CQ メールサービスの設定](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=ja#configuring-the-mail-service)
* 有効な電子メール ID をに関連付ける [管理者ユーザー](http://localhost:4502/security/users.html)
* をダウンロードしてインストールする [ワークフローと通知テンプレート](assets/workflow-and-task-notification-template.zip) using [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* ダウンロード [アダプティブフォーム](assets/request-travel-authorization.zip) からAEMにインポートします。 [フォームとドキュメントの ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* をデプロイして起動します。 [カスタムバンドル](assets/work-items-user-service-bundle.jar) の使用 [web コンソール](http://localhost:4502/system/console/bundles)
* [フォームをプレビューして送信する](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

フォーム送信タスクの割り当て通知が、管理者ユーザーに関連付けられた電子メール ID に送信されます。 次のスクリーンショットは、タスク割り当て通知の例を示しています

![通知](assets/task-nitification-email.png)

>[!NOTE]
>タスクの割り当て通知用の電子メールテンプレートは、次の形式にする必要があります。
>
> subject=割り当てられたタスク — `${workitem_title}`
>
> message=改行文字を含まない電子メールテンプレートを表す文字列。

## タスクの割り当て電子メール通知のタスクコメント

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

上記のコードを含むバンドルは、 [ここからダウンロード](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)
