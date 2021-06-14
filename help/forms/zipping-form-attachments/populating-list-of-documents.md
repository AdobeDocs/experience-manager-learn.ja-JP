---
title: リスト変数を入力するカスタムプロセス手順
description: ドキュメント型と文字列型のリスト変数を入力するカスタムプロセス手順
feature: ワークフロー
topic: 開発
version: 6.5
role: Developer
level: Beginner
kt: kt-8063
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 1%

---


# カスタムプロセスステップ


配列リストワークフロー変数を設定するためのカスタムプロセス手順が実装されました。 OSGiバンドルの作成に慣れていない場合は、[次の手順に従ってください。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)

カスタムプロセスステップのコードは、次の処理を行います

* ペイロードフォルダーの下にあるすべてのアダプティブフォームの添付ファイルに対してクエリを実行します。 フォルダー名はプロセス引数としてプロセスステップに渡されます。

* `listOfDocuments`ワークフロー変数を設定する
* `attachmentNames`ワークフロー変数を設定する
* ワークフロー変数の値を設定する(`no_of_attachments`)

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
> コードが機能するために、ワークフローで定義されている次の変数があることを確認してください
> *listOfDocuments* - ArrayList of Documents型の変数
> *attachmentNames* - StringのArrayList型の変数
> *no_of_attachments* - Double型の変数
