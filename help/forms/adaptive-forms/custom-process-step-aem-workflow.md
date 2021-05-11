---
title: カスタムプロセス手順の実装
seo-title: カスタムプロセス手順の実装
description: カスタムプロセス手順を使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
seo-description: カスタムプロセス手順を使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: ワークフロー
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 4%

---


# カスタムプロセス手順

このチュートリアルは、カスタムプロセス手順を導入するAEM Formsのお客様を対象としています。 プロセスステップでは、ECMAスクリプトを実行したり、カスタムJavaコードを呼び出して操作を実行したりできます。 このチュートリアルでは、プロセスステップによって実行されるWorkflowProcessを実装するために必要な手順を説明します。

カスタムプロセス手順を実装する主な理由は、AEMワークフローを拡張することです。 例えば、ワークフローモデルでAEM Formsコンポーネントを使用している場合は、次の操作を実行できます

* アダプティブフォームの添付ファイルをファイルシステムに保存する
* 送信データを操作する

上記の使用例を達成するには、通常、プロセス手順で実行されるOSGiサービスを作成します。

## Mavenプロジェクトの作成

最初の手順は、適切なAdobeMavenアーキタイプを使用してMavenプロジェクトを作成することです。 詳細な手順は、この[記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en)に記載されています。 MavenプロジェクトをEclipseにインポートしたら、プロセス手順で使用できる最初のOSGiコンポーネントを書き込む開始の準備が整います。


### WorkflowProcessを実装するクラスを作成します

Eclipse IDEでMavenプロジェクトを開きます。 **projectname** > **core**フォルダーを展開します。 src/main/javaフォルダーを展開します。 「core」で終わるパッケージが表示されます。 このパッケージにWorkflowProcessを実装するJavaクラスを作成します。 executeメソッドをオーバーライドする必要があります。 executeメソッドのシグネチャは次のとおりです。
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException
executeメソッドは、次の3つの変数にアクセスします

**WorkItem**:workItem変数は、ワークフローに関連するデータへのアクセスを提供します。パブリックAPIドキュメントは[こちらから入手できます。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:このworkflowSession変数を使用すると、ワークフローを制御できます。パブリックAPIドキュメントは、[こちら](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)で入手できます

**MetaDataMap**:ワークフローに関連付けられているすべてのメタデータ。プロセスステップに渡されるプロセス引数は、MetaDataMapオブジェクトを使用して使用できます。[API に関するドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html?lang=ja)

このチュートリアルでは、アダプティブフォームに追加された添付ファイルをAEMワークフローの一部としてファイルシステムに書き込みます。

この使用例を達成するために、次のJavaクラスが記述されています

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

1行目 — コンポーネントのプロパティを定義します。 process.labelプロパティは、次のスクリーンショットの1つに示すように、OSGiコンポーネントをプロセス手順に関連付けるときに表示される内容です。

13 ～ 15行目 — このOSGiコンポーネントに渡されるプロセス引数は、「,」区切り文字を使用して分割されます。 次に、attachmentPathとsaveToLocationの値を文字列配列から抽出します。

* attachmentPath - AEMワークフローを呼び出すようにアダプティブフォームの送信アクションを設定した場合に、アダプティブフォームで指定したのと同じ場所です。 これは、ワークフローのペイロードに対するAEMでの添付ファイルの保存を希望するフォルダーの名前です。

* saveToLocation - AEMサーバーのファイルシステム上で添付ファイルを保存する場所です。

これらの2つの値は、次のスクリーンショットに示すように、プロセスの引数として渡されます。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilderサービスは、attachmentsPathフォルダーの下にnt:fileタイプのノードをクエリするために使用されます。 残りのコードは検索結果を繰り返し処理し、ドキュメントオブジェクトを作成してファイルシステムに保存します


>[!NOTE]
>
>AEM Formsに固有のドキュメントオブジェクトを使用するので、mavenプロジェクトにaemfd-client-sdkの依存関係を含める必要があります。 グループIDはcom.adobe.aemfdで、アーティファクトIDはaemfd-client-sdkです。

#### 構築と導入

[説明に従ってバンドルを構築します。バンドルが展開され、アクティブな状態であることを確認します。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[](http://localhost:4502/system/console/bundles)

ワークフローモデルを作成する. ワークフローモデルにプロセスステップをドラッグ&amp;ドロップします。 プロセス手順を「アダプティブフォームの添付ファイルをファイルシステムに保存」に関連付けます。

必要なプロセス引数をコンマで区切って指定します。 例えば、Attachments,c:\\scrappp\\. 最初の引数は、アダプティブフォーム添付ファイルがワークフローのペイロードを基準に保存される際のフォルダーです。 これは、アダプティブフォームの送信アクションを設定する際に指定した値と同じである必要があります。 2番目の引数は、添付ファイルを保存する場所です。

アダプティブフォームの作成. 「添付ファイル」コンポーネントをフォームにドラッグ&amp;ドロップします。 前の手順で作成したワークフローを呼び出すように、フォームの送信アクションを設定します。 適切な添付ファイルパスを指定します。

設定を保存します。

フォームをプレビューする. 2追加つの添付ファイルを作成し、フォームを送信します。 添付ファイルは、ワークフロー内で指定した場所のファイルシステムに保存されます。

