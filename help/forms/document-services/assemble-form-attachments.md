---
title: フォーム添付ファイルのアセンブリ
description: 指定された順序でフォーム添付ファイルをアセンブリする
feature: Assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# フォーム添付ファイルのアセンブリ

この記事では、アダプティブフォームの添付ファイルを指定された順序で組み立てるためのアセットを提供します。 このサンプルコードを使用するには、フォームの添付ファイルをPDF形式にする必要があります。 使用例を次に示します。
アダプティブフォームに入力するユーザーは、1つ以上のpdfドキュメントをフォームに添付します。
フォーム送信時に、フォームの添付ファイルを組み立てて1つのPDFを生成します。 添付ファイルをアセンブリして最終的なPDFを生成する順序を指定できます。

## WorkflowProcessインターフェイスを実装するOSGiコンポーネントの作成

[com.adobe.granite.workflow.exec.WorkflowProcessインターフェイス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html)を実装するOSGiコンポーネントを作成します。 このコンポーネント内のコードは、AEMワークフローのプロセスステップコンポーネントに関連付けることができます。 このコンポーネントには、インターフェイスcom.adobe.granite.workflow.exec.WorkflowProcessのexecuteメソッドが実装されています。

アダプティブフォームがAEMワークフローのトリガーに送信されると、送信されたデータはペイロードフォルダーの下の指定されたファイルに保存されます。 例えば、これは送信済みデータファイルです。 idcardタグとbankstatementsタグで指定された添付ファイルを組み立てる必要があります。
![submitted-data](assets/submitted-data.JPG)を参照してください。

### タグ名の取得

添付ファイルの順序は、以下のスクリーンショットに示すように、ワークフローのプロセスステップ引数として指定されます。 ここでは、フィールドidカードに追加された添付ファイルを組み立て、次にbankstatementsを組み立てます。

![process-step](assets/process-step.JPG)

次のコードスニペットは、プロセス引数から添付ファイル名を抽出します

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 添付ファイル名からのDDXの作成

次に、Assemblerサービスでドキュメントをアセンブリするために使用する[Document Description XML(DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)ドキュメントを作成する必要があります。 次に、プロセス引数から作成されたDDXを示します。 NoForms要素を使用すると、XFAベースのドキュメントをアセンブリする前に統合できます。 PDFソース要素は、プロセス引数で指定された順番で表示されます。

![ddx-xml](assets/ddx.PNG)

### ドキュメントのマップの作成

次に、添付ファイル名をキーに、添付ファイルを値に持つドキュメントのマップを作成します。 Query Builderサービスは、ペイロードパスの下の添付ファイルに対してクエリを実行し、ドキュメントのマップを作成するために使用されました。 このドキュメントマップとDDXは、Assemblerサービスが最終的なPDFをアセンブリするために必要です。

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

### AssemblerServiceを使用したドキュメントのアセンブリ

DDXとドキュメントマップを作成した後、次の手順では、 AssemblerServiceを使用してドキュメントをアセンブリします。
次のコードは、アセンブリされたpdfをアセンブルして返します。

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

### ペイロードフォルダーの下にアセンブリ済みのPDFを保存する

最後の手順は、ペイロードフォルダーの下にアセンブリされたpdfを保存することです。 このPDFには、後続の処理手順でアクセスできます。
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

次に、フォーム添付ファイルがアセンブルされて保存された後のペイロードフォルダー構造を示します。

![payload-structure](assets/payload-structure.JPG)

### この機能をAEM Serverで動作させるには

* [Assemble Form Attachments Form](assets/assemble-form-attachments-af.zip)をローカルシステムにダウンロードします。
* [Formsとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)ページからフォームを読み込みます。
* [workflow](assets/assemble-form-attachments.zip)をダウンロードし、パッケージマネージャーを使用してAEMに読み込みます。
* [カスタムバンドル](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)をダウンロードします。
* [Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイし、起動します。
* ブラウザーで[AssembleAttachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)を参照します。
* IDドキュメントに添付ファイルを追加し、銀行取引明細書セクションにいくつかのPDFドキュメントを追加します
* フォームを送信してワークフローをトリガーする
* crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload)内のワークフローの[payloadフォルダーで、アセンブリ済みのPDFを確認します。

>[!NOTE]
> カスタムバンドルのロガーを有効にしている場合、DDXとアセンブリ済みのファイルがAEMインストールのフォルダーに書き込まれます。

