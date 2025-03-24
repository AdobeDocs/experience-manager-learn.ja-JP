---
title: カスタムプロセスステップの実装
description: カスタムプロセスステップを使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 100%

---

# カスタムプロセスステップ

このチュートリアルは、カスタムプロセスステップを実装する必要がある AEM Forms のお客様を対象としています。プロセスステップでは、ECMA スクリプトを実行したり、カスタム Java™ コードを呼び出して操作を実行できます。このチュートリアルでは、プロセスステップで実行される WorkflowProcess の実装に必要な手順を説明します。

カスタムプロセスステップを実装する主な理由は、AEM ワークフローを拡張することです。例えば、ワークフローモデルで AEM Forms コンポーネントを使用している場合は、次の操作の実行が必要なことがあります。

* アダプティブフォームの添付ファイルをファイルシステムに保存する
* 送信されたデータを操作する

上記のユースケースを実現するには、通常、プロセスステップで実行される OSGi サービスを記述します。

## Maven プロジェクトの作成

最初の手順は、適切な Adobe Maven アーキタイプを使用して Maven プロジェクトを作成することです。詳細な手順について詳しくは、この[記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=ja)を参照してください。Maven プロジェクトを Eclipse に読み込んだら、プロセスステップで使用できる最初の OSGi コンポーネントの記述を開始する準備が整います。


### WorkflowProcess を実装するクラスの作成

Eclipse IDE で Maven プロジェクトを開きます。**[projectname]**／**core** フォルダーを展開します。`src/main/java` フォルダーを展開します。`core` で終わるパッケージが表示されます。このパッケージに、WorkflowProcess を実装する Java™ クラスを作成します。execute メソッドをオーバーライドする必要があります。execute メソッドのシグネチャは次のとおりです。

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

execute メソッドを使用すると、次の 3 つの変数にアクセスできます。

**WorkItem**：workItem 変数は、ワークフローに関連するデータにアクセスできるようにします。 公開 API のドキュメントは[こちら](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html?lang=ja)で参照できます。 

**WorkflowSession**：workflowSession 変数を使用すると、ワークフローを制御できます。公開 API のドキュメントについて詳しくは、[こちら](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html?lang=ja)を参照してください。

**MetaDataMap**：ワークフローに関連付けられているすべてのメタデータです。 プロセスステップに渡されるプロセス引数は、 MetaDataMap オブジェクトを使用して参照できます。[API に関するドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

このチュートリアルでは、アダプティブフォームに追加された添付ファイルを AEM ワークフローの一環としてファイルシステムに書き込みます。

このユースケースを実現することを目的に、次の Java™ クラスが記述されました。

このコードを見てみましょう。

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

1 行目 - コンポーネントのプロパティを定義します。`process.label` プロパティは、OSGi コンポーネントをプロセスステップに関連付ける際に表示されるものです（次のスクリーンショットの 1 つを参照）。

13～15 行目 - この OSGi コンポーネントに渡されるプロセス引数は、区切り記号「,」を使用して分割されます。 次に、 attachmentPath と saveToLocation の値が文字列配列から抽出されます。

* attachmentPath - AEM Workflow を呼び出すようにアダプティブフォームの送信アクションを設定したときに、アダプティブフォームで指定したのと同じ場所です。 これは、添付ファイルを保存する AEM 内のフォルダーの名前（ワークフローのペイロードを基準とする相対パス）です。

* saveToLocation - AEM サーバーのファイルシステム上で添付ファイルを保存する場所です。

これら 2 つの値がプロセス引数として渡されます（以下のスクリーンショットを参照）。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder サービスは、attachmentsPath フォルダー下の `nt:file` タイプのノードに対してクエリを実行するのに使用します。残りのコードでは、検索結果を反復処理して Document オブジェクトを作成し、それをファイルシステムに保存します。


>[!NOTE]
>
>AEM Forms に固有の Document オブジェクトを使用しているので、aemfd-client-sdk の依存関係を Maven プロジェクトに含める必要があります。グループ ID は `com.adobe.aemfd` で、アーティファクト ID は `aemfd-client-sdk` です。

#### ビルドとデプロイ

[ここで説明しているとおりに、バンドルをビルドします。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=ja)
[バンドルがデプロイされ、アクティブな状態になっていることを確認します。](http://localhost:4502/system/console/bundles)

ワークフローモデルを作成します。プロセスステップをワークフローモデルにドラッグ＆ドロップします。 プロセスステップを「アダプティブフォームの添付ファイルをファイルシステムに保存」に関連付けます。

必要なプロセス引数をコンマで区切って指定します。 例えば、「Attachments,c:\\scrappp\\」などです。 最初の引数は、アダプティブフォームの添付ファイルが保存される際のフォルダー（ワークフローのペイロードを基準とする相対パス）です。 これは、アダプティブフォームの送信アクションを設定する際に指定した値と同じにする必要があります。 2 番目の引数は、添付ファイルを保存する場所です。

アダプティブフォームを作成します。添付ファイルコンポーネントをフォームにドラッグ＆ドロップします。 前の手順で作成したワークフローを呼び出すように、フォームの送信アクションを設定します。 適切な添付ファイルパスを指定します。

設定を保存します。

フォームをプレビューします。いくつかの添付ファイルを追加し、フォームを送信します。 添付ファイルは、ファイルシステムの、ワークフローで指定した場所に保存される必要があります。
