---
title: リスト変数に値を入力するカスタムプロセスステップ
description: ドキュメントタイプと文字列タイプのリスト変数に値を入力するカスタムプロセスステップ
feature: Workflow
topic: Development
version: 6.5
role: Developer
level: Beginner
kt: kt-8063
exl-id: 09d9eabf-4815-4159-b6c7-cf2ebc8a2df5
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: ht
source-wordcount: '139'
ht-degree: 100%

---

# カスタムプロセスステップ


配列リストワークフロー変数に値を入力するためのカスタムプロセスステップを実装しました。 OSGi バンドルの作成に詳しくない場合は、[次の手順に従ってください](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=ja)。

カスタムプロセスステップのコードでは、以下を行います。

* ペイロードフォルダーにあるすべてのアダプティブフォーム添付ファイルをクエリします。 フォルダー名がプロセス引数としてプロセスステップに渡されます。

* `listOfDocuments` ワークフロー変数に値を入力します。
* `attachmentNames` ワークフロー変数に値を入力します。
* ワークフロー変数（`no_of_attachments`）の値を設定します。

```java
 package com.aemforms.formattachments.core;

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
        Constants.SERVICE_DESCRIPTION + "=PopulateListOfDocuments",
        "process.label" + "=PopulateListOfDocuments"
})

public class PopulateListOfDocuments implements WorkflowProcess {

        private static final Logger log = LoggerFactory.getLogger(PopulateListOfDocuments.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path  is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                int no_of_attachments = result.getHits().size();
                Document[] listOfDocuments = new Document[no_of_attachments];
                String[] attachmentNames = new String[no_of_attachments];
                int i = 0;
                for (Hit hit: result.getHits()) {
                        try {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                listOfDocuments[i] = new Document(attachmentStream);
                                attachmentNames[i] = new String(hit.getTitle());
                                log.debug("Added " + hit.getTitle() + "to the list");
                                i++;
                        } catch (Exception e) {
                                log.error("Unable to obtain attachment", e);
                        }
                }

                metaDataMap.put("no_of_attachments", no_of_attachments);
                metaDataMap.put("listOfDocuments", listOfDocuments);
                metaDataMap.put("attachmentNames", attachmentNames);

                log.debug("Updated workflow");
        }

}
```

>[!NOTE]
>
> コードが機能するために、次の変数がワークフローで必ず定義されるようにしてください。
> *listOfDocuments* - ドキュメントの ArrayList タイプの変数
> *attachmentNames* - 文字列の ArrayList タイプの変数
> *no_of_attachments* - Double タイプの変数
