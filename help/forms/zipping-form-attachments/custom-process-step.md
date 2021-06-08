---
title: 添付ファイルをzipするカスタムプロセスステップ
description: アダプティブフォームの添付ファイルをzipファイルに追加し、このzipファイルをワークフロー変数に保存するカスタムプロセスステップ
sub-product: フォーム[ふぉーむ]
feature: ワークフロー
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---


# カスタムプロセスステップ


フォームの添付ファイルを含むzipファイルを作成するためのカスタムプロセスステップが実装されました。 OSGiバンドルの作成に慣れていない場合は、[次の手順に従ってください。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)

カスタムプロセスステップのコードは、次の処理を行います

* ペイロードフォルダーの下にあるすべてのアダプティブフォームの添付ファイルに対してクエリを実行します。 フォルダー名はプロセス引数としてプロセスステップに渡されます。
* zipファイルを作成し、フォームの添付ファイルをzipファイルに追加します。
* 2つのワークフロー変数の値を設定します（attachments_zipおよびno_of_attachments）。

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

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
import com.day.cq.search.result.SearchResult;;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})

public class CreateFormAttachmentCopy implements WorkflowProcess {
        private static final Logger log = LoggerFactory.getLogger(CreateFormAttachmentCopy.class);
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
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                ZipOutputStream zipOut = new ZipOutputStream(baos);
                int no_of_attachments = result.getHits().size();
                for (Hit hit: result.getHits())
                {
                        try
                        {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                                int nRead;
                                byte[] data = new byte[1024];
                                while ((nRead = attachmentStream.read(data, 0, data.length)) != -1)
                                {
                                        buffer.write(data, 0, nRead);
                                }

                                buffer.flush();
                                byte[] byteArray = buffer.toByteArray();
                                ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                                zipOut.putNextEntry(zipEntry);
                                zipOut.write(byteArray);
                                zipOut.closeEntry();

                        } 
                        catch (Exception e)
                        {
                                log.debug("The error message is " + e.getMessage());
                        }
                }
                try
                {
                        zipOut.close();

                }
                catch (IOException e)
                {
                        
                        log.debug(("Error in closing zipout" + e.getMessage()));
                }

                // set the value of the workflow variables.
                metaDataMap.put("attachments_zip", new Document(baos.toByteArray()));
                metaDataMap.put("no_of_attachments", no_of_attachments);

                workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                log.debug("Updated workflow");

        }

}
```

>[!NOTE]
>
> このコードが機能するには、ワークフローで、ドキュメントタイプの&#x200B;*attachments_zip*&#x200B;と、添付ファイルタイプの&#x200B;*no_of_attachments*&#x200B;という変数があることを確認してください


