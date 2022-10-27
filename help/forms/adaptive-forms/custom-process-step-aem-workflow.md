---
title: カスタムプロセスの実装手順
description: カスタムプロセスステップを使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 4%

---

# カスタムプロセスステップ

このチュートリアルは、AEM Formsのお客様がカスタムプロセス手順を実装する必要がある場合を対象としています。 プロセスステップでは、ECMA スクリプトを実行したり、カスタム Java コードを呼び出して操作を実行したりできます。 このチュートリアルでは、プロセスステップで実行される WorkflowProcess の実装に必要な手順を説明します。

カスタムプロセスの手順を実装する主な理由は、AEM Workflow を拡張することです。 例えば、ワークフローモデルでAEM Formsコンポーネントを使用している場合、次の操作を実行することができます

* アダプティブフォームの添付ファイルをファイルシステムに保存する
* 送信されたデータの操作

上記の使用例を達成するには、通常、プロセスステップで実行される OSGi サービスを記述します。

## Maven プロジェクトを作成

最初の手順は、適切なAdobeMaven アーキタイプを使用して Maven プロジェクトを作成することです。 詳細な手順を次に示します [記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Maven プロジェクトを Eclipse に読み込んだら、プロセスステップで使用できる最初の OSGi コンポーネントの記述を開始する準備が整います。


### WorkflowProcess を実装するクラスを作成します

Eclipse IDE で Maven プロジェクトを開きます。 展開 **projectname** > **コア** フォルダー。 src/main/java フォルダーを展開します。 「core」で終わるパッケージが表示されます。 このパッケージで、WorkflowProcess を実装する Java クラスを作成します。 execute メソッドを上書きする必要があります。 execute メソッドのシグネチャは、次の 3 つの変数にアクセスできるようになります。

**WorkItem**:workItem 変数は、ワークフローに関連するデータにアクセスできるようにします。 公開 API ドキュメントを参照できます。 [こちら。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:この workflowSession 変数を使用すると、ワークフローを制御できます。 公開 API ドキュメントを参照できます。 [ここ](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**:ワークフローに関連付けられているすべてのメタデータ。 プロセスステップに渡されるプロセス引数は、 MetaDataMap オブジェクトを使用して使用できます。[API に関するドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

このチュートリアルでは、アダプティブフォームに追加された添付ファイルをAEM Workflow の一部としてファイルシステムに書き込みます。

この使用例を達成するために、次の Java クラスが記述されました

このコードを見てみましょう

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

行 1 — コンポーネントのプロパティを定義します。 process.label プロパティは、次のスクリーンショットの 1 つに示すように、OSGi コンポーネントをプロセスステップに関連付けるときに表示される内容です。

13～15 行目 — この OSGi コンポーネントに渡されるプロセス引数は、「,」区切り文字を使用して分割されます。 次に、 attachmentPath と saveToLocation の値が文字列配列から抽出されます。

* attachmentPath — これは、AEM Workflow を呼び出すようにアダプティブフォームの送信アクションを設定したときに、アダプティブフォームで指定したのと同じ場所です。 これは、ワークフローのペイロードを基準に、AEMで添付ファイルを保存するフォルダーの名前です。

* saveToLocation - AEMサーバーのファイルシステム上で添付ファイルを保存する場所です。

これらの 2 つの値は、以下のスクリーンショットに示すように、プロセス引数として渡されます。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder サービスは、attachmentsPath フォルダーの下の nt:file 型のノードに対してクエリを実行するために使用します。 残りのコードは、検索結果を繰り返し処理して Document オブジェクトを作成し、ファイルシステムに保存します


>[!NOTE]
>
>AEM Formsに固有の Document オブジェクトを使用するので、aemfd-client-sdk 依存関係を Maven プロジェクトに含める必要があります。 グループ ID は com.adobe.aemfd で、アーティファクト ID は aemfd-client-sdk です。

#### ビルドとデプロイ

[ここで説明されているように、バンドルをビルドします。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[バンドルがデプロイされ、アクティブな状態になっていることを確認します。](http://localhost:4502/system/console/bundles)

ワークフローモデルの作成. プロセスステップをワークフローモデルにドラッグ&amp;ドロップします。 プロセスステップを「アダプティブフォームの添付ファイルをファイルシステムに保存」に関連付けます。

必要なプロセス引数をコンマで区切って指定します。 例えば、添付ファイル、c:\\scrappp\\などです。 最初の引数は、アダプティブフォームの添付ファイルがワークフローのペイロードを基準に保存される際のフォルダーです。 これは、アダプティブフォームの送信アクションを設定する際に指定した値と同じである必要があります。 2 番目の引数は、添付ファイルを保存する場所です。

アダプティブフォームの作成. 添付ファイルコンポーネントをフォームにドラッグ&amp;ドロップします。 前の手順で作成したワークフローを呼び出すように、フォームの送信アクションを設定します。 適切な添付ファイルのパスを指定します。

設定を保存します。

フォームをプレビューする. いくつかの添付ファイルを追加し、フォームを送信します。 添付ファイルは、ワークフロー内で指定された場所のファイルシステムに保存される必要があります。
