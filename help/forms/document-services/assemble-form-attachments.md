---
title: フォーム添付ファイルのアセンブリ
description: 指定された順序でフォーム添付ファイルをアセンブリ
feature: Assembler
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 1%

---

# フォーム添付ファイルのアセンブリ

この記事では、アダプティブフォームの添付ファイルを指定された順序で組み立てるためのアセットを提供します。 このサンプルコードを機能させるには、フォームの添付ファイルを pdf 形式にする必要があります。 次に使用例を示します。
アダプティブフォームに入力するユーザーは、1 つ以上の pdf ドキュメントをフォームに添付します。
フォーム送信時に、フォームの添付ファイルを組み立てて 1 つの PDF を生成します。 最終的な PDF を生成するために添付ファイルを組み立てる順序を指定できます。

## WorkflowProcess インターフェイスを実装する OSGi コンポーネントの作成

を実装する OSGi コンポーネントを作成する [com.adobe.granite.workflow.exec.WorkflowProcess インターフェイス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). このコンポーネント内のコードは、AEMワークフローのプロセスステップコンポーネントに関連付けることができます。 このコンポーネントには、インターフェイス com.adobe.granite.workflow.exec.WorkflowProcess の execute メソッドが実装されています。

アダプティブフォームがAEMワークフローのトリガーに送信されると、送信されたデータは、ペイロードフォルダーの下の指定されたファイルに保存されます。 例えば、これは送信されたデータファイルです。 idcard および bankstatements タグで指定された添付ファイルを組み立てる必要があります。
![submitted-data](assets/submitted-data.JPG).

### タグ名を取得する

添付ファイルの順序は、以下のスクリーンショットに示すように、ワークフローでプロセスステップ引数として指定されます。 ここでは、フィールド id カードに追加された添付ファイルを組み立て、次に bankstatements を組み立てます。

![process-step](assets/process-step.JPG)

次のコードスニペットは、プロセス引数から添付ファイル名を抽出します

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 添付ファイル名から DDX を作成

次に、 [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) ドキュメントのアセンブリに Assembler サービスで使用されるドキュメント。 以下は、プロセス引数から作成された DDX です。 NoForms 要素を使用すると、XFA ベースのドキュメントをアセンブリする前に統合できます。 PDFソース要素は、プロセス引数で指定された順番で正しく配置されています。

![ddx-xml](assets/ddx.PNG)

### ドキュメントのマップを作成

次に、添付ファイル名をキーにし、値に添付ファイルを含むドキュメントのマップを作成します。 Query Builder サービスは、ペイロードパスの下の添付ファイルに対してクエリを実行し、ドキュメントのマップを作成するために使用されました。 このドキュメントのマップと DDX は、Assembler サービスが最終的な PDF をアセンブリするために必要です。

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

### AssemblerService を使用したドキュメントのアセンブリ

DDX とドキュメントマップが作成された後、次の手順では、 AssemblerService を使用してドキュメントをアセンブリします。
次のコードは、アセンブルされた pdf をアセンブルして返します。

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

### ペイロードフォルダーの下にアセンブリされた PDF を保存します。

最後の手順は、ペイロードフォルダーの下にアセンブリされた pdf を保存することです。 その後、ワークフローの後続の手順でこの PDF にアクセスして、さらに処理をおこなうことができます。
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

次に、フォーム添付ファイルが組み立てられて保存された後のペイロードフォルダー構造を示します。

![payload-structure](assets/payload-structure.JPG)

### この機能をAEM Server で動作させるには

* をダウンロードします。 [フォーム添付ファイルフォームのアセンブリ](assets/assemble-form-attachments-af.zip) をローカルシステムに送信します。
* フォームを[Forms And Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) ページ。
* ダウンロード [ワークフロー](assets/assemble-form-attachments.zip) パッケージマネージャーを使用してAEMに読み込みます。
* をダウンロードします。 [カスタムバンドル](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* を使用してバンドルをデプロイおよび開始します。 [web コンソール](http://localhost:4502/system/console/bundles)
* ブラウザーで次の場所を指定します。 [AssembleAttachments フォーム](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* ID ドキュメントに添付ファイルを追加し、いくつかの PDF ドキュメントを銀行取引明細書セクションに追加します
* フォームを送信してワークフローをトリガー
* ワークフローの [crx のペイロードフォルダー](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) アセンブリ済み pdf の場合

>[!NOTE]
> カスタムバンドルのロガーを有効にしている場合、DDX とアセンブリ済みのファイルはAEMインストール環境のフォルダーに書き込まれます。
