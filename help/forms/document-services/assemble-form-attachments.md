---
title: フォーム添付ファイルのアセンブリ
description: 指定された順序でフォームの添付ファイルを組み合わせます
feature: Assembler
version: 6.4,6.5
jira: KT-6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
duration: 150
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '589'
ht-degree: 100%

---

# フォーム添付ファイルのアセンブリ

この記事では、指定された順序でアダプティブフォームの添付ファイルを組み合わせるためのアセットを提供します。このサンプルコードを機能させるには、フォームの添付ファイルを PDF 形式にします。次にユースケースを示します。
ユーザーがアダプティブフォームに入力すると、フォームに 1 つ以上の PDF ドキュメントが添付されます。
フォーム送信時に、フォームの添付ファイルを組み合わせて 1 つの PDF を生成します。最終的な PDF を生成するために組み合わせる添付ファイルの順序を指定できます。

## WorkflowProcess インターフェイスを実装する OSGi コンポーネントの作成

[com.adobe.granite.workflow.exec.WorkflowProcess インターフェイス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html?lang=ja)を実装する OSGi コンポーネントを作成します。このコンポーネントのコードは、AEM ワークフローのプロセスステップコンポーネントに関連付けることができます。このコンポーネントには、com.adobe.granite.workflow.exec.WorkflowProcess インターフェイスの実行メソッドが実装されています。

AEM ワークフローをトリガーするためにアダプティブフォームが送信されると、送信されたデータはペイロードフォルダーの下にある指定されたファイルに保存されます。例えば、次のようなデータファイルが送信されます。idcard タグと bankstatements タグで指定された添付ファイルを組み合わせる必要があります。
![submitted-data](assets/submitted-data.JPG)

### タグ名の取得

以下のスクリーン ショットに示すように、添付ファイルの順序は、ワークフローのプロセスステップ引数として指定されます。ここでは、idcard フィールドに追加された添付ファイル、bankstatements フィールドに追加された添付ファイルの順にアセンブリします。

![process-step](assets/process-step.JPG)

次のコードスニペットは、プロセス引数から添付ファイル名を抽出します

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 添付ファイル名から DDX を作成

次に、ドキュメントを組み合わせるために Assembler サービスで使用される [Document Description XML（DDX）](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)ドキュメントを作成します。以下は、プロセス引数から作成された DDX です。NoForms 要素を使用すると、XFA ベースのドキュメントを作成する前に統合できます。PDF ソース要素は、プロセス引数で指定された順番で正しく配置されます。

![ddx-xml](assets/ddx.PNG)

### ドキュメントのマップを作成

次に、添付ファイル名をキー、添付ファイルを値としてドキュメントのマップを作成します。Query Builder サービスを使用して、ペイロードパスの下にある添付ファイルに対してクエリを実行し、ドキュメントのマップを作成しました。このドキュメントのマップと DDX は、Assembler サービスで最終的な PDF を作成するために必要です。

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### Assembler サービスを使用したドキュメントのアセンブリ

DDX とドキュメントマップが作成されたら、次の手順では Assembler サービスを使用してドキュメントを作成します。
次のコードは、DF を組み合わせ、組み合わせられた PDF を返します。

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### ペイロードフォルダーの下に作成した PDF を保存します。

最後の手順では、組み合わせた PDF をペイロードフォルダーに保存します。ワークフローの後続の手順でこの PDF にアクセスして、さらに処理することができます。
次のコードスニペットを使用して、ペイロードフォルダーの下にファイルを保存しました

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

以下に、フォーム添付ファイルを組み合わせて保存した後のペイロードフォルダー構造を示します。

![payload-structure](assets/payload-structure.JPG)

### この機能を AEM Server で動作させるには

* [フォーム添付ファイルのアセンブリフォーム](assets/assemble-form-attachments-af.zip)をローカルシステムにダウンロードします。
* [フォームとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)ページからフォームを読み込みます。
* [ワークフロー](assets/assemble-form-attachments.zip)をダウンロードし、パッケージマネージャーを使用して AEM に読み込みます。
* [カスタムバンドル](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)をダウンロードします
* [Web コンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイして開始します
* ブラウザーで[添付ファイルのアセンブリフォーム](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)にアクセスします
* ID ドキュメントに添付ファイルを追加し、いくつかの PDF ドキュメントを銀行取引明細書セクションに追加します
* フォームを送信してワークフローをトリガーします
* 組み合わせた PDF の[ crx にあるワークフローのペイロードフォルダー](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload)を確認します

>[!NOTE]
> カスタムバンドルのロガーを有効にしている場合、DDX 、および組み合わせたファイルは AEM インストールのフォルダーに書き込まれます。
