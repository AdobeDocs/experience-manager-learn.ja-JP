---
title: ダイアログを含むカスタムプロセスステップの実装
description: カスタムプロセスステップを使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 1%

---

# カスタムプロセスステップ

このチュートリアルは、カスタムワークフローコンポーネントの実装が必要なAEM Formsのお客様を対象としています。ワークフローコンポーネントの作成の最初の手順は、ワークフローコンポーネントに関連付けられる Java コードを記述することです。 このチュートリアルでは、アダプティブフォームの添付ファイルをファイルシステムに保存する簡単な java クラスを記述します。この Java コードは、ワークフローコンポーネントで指定された引数を読み取ります。

次の手順は、Java クラスを記述し、クラスを OSGi バンドルとしてデプロイするために必要です

## Maven プロジェクトを作成

最初の手順は、適切なAdobeMaven アーキタイプを使用して Maven プロジェクトを作成することです。 詳細な手順を次に示します [記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Maven プロジェクトを Eclipse に読み込んだら、プロセスステップで使用できる最初の OSGi コンポーネントの記述を開始する準備が整います。


### WorkflowProcess を実装するクラスを作成します

Eclipse IDE で Maven プロジェクトを開きます。 展開 **projectname** > **コア** フォルダー。 src/main/java フォルダーを展開します。 「core」で終わるパッケージが表示されます。 このパッケージで、WorkflowProcess を実装する Java クラスを作成します。 execute メソッドを上書きする必要があります。 execute メソッドの署名は、パブリック void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throwsWorkflowException

このチュートリアルでは、アダプティブフォームに追加された添付ファイルをAEM Workflow の一部としてファイルシステムに書き込みます。

この使用例を達成するために、次の Java クラスが記述されました

このコードを見てみましょう

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);
<<<<<<< HEAD
    log.debug("Got Attachments PAth" + attachmentsPath);
    String saveToLocation = metaDataMap.get("saveToLocation", String.class);
=======
    log.debug("Got Attachments Path" + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
>>>>>>> 1d85e32dec060b5f5f33607225cd6ceb8d2b21c5
    log.debug("Got Save Location" + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath - AEM Workflow を呼び出すようにアダプティブフォームの送信アクションを設定したときに、アダプティブフォームで指定したのと同じ場所です。 これは、ワークフローのペイロードを基準に、AEMで添付ファイルを保存するフォルダーの名前です。

* saveToLocation - AEMサーバーのファイルシステム上で添付ファイルを保存する場所です。

これら 2 つの値は、ワークフローコンポーネントのダイアログを使用してプロセス引数として渡されます

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder サービスは、attachmentsPath フォルダーの下の nt:file 型のノードに対してクエリを実行するために使用します。 残りのコードは、検索結果を繰り返し処理して Document オブジェクトを作成し、ファイルシステムに保存します


>[!NOTE]
>
>AEM Formsに固有の Document オブジェクトを使用するので、aemfd-client-sdk 依存関係を Maven プロジェクトに含める必要があります。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### ビルドとデプロイ

[ここで説明されているように、バンドルをビルドします。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[バンドルがデプロイされ、アクティブな状態になっていることを確認します。](http://localhost:4502/system/console/bundles)

