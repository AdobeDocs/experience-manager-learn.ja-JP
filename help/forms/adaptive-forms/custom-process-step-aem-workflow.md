---
title: カスタムプロセス手順の実装
seo-title: カスタムプロセス手順の実装
description: カスタムプロセス手順を使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
seo-description: カスタムプロセス手順を使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 2%

---


# カスタムプロセス手順

このチュートリアルは、カスタムプロセス手順を導入するAEM Formsのお客様を対象としています。 プロセスステップでは、ECMAスクリプトを実行したり、カスタムJavaコードを呼び出して操作を実行したりできます。 このチュートリアルでは、プロセスステップによって実行されるWorkflowProcessを実装するために必要な手順を説明します。

カスタムプロセス手順を実装する主な理由は、AEMワークフローを拡張することです。 例えば、ワークフローモデルでAEM Formsコンポーネントを使用している場合は、次の操作を実行できます

* アダプティブフォームの添付ファイルをファイルシステムに保存する
* 送信データを操作する

上記の使用例を達成するには、通常、プロセス手順で実行されるOSGiサービスを作成します。

## Mavenプロジェクトの作成

最初の手順は、適切なAdobeMavenアーキタイプを使用してMavenプロジェクトを作成することです。 詳細な手順は、この[記事](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)に記載されています。 MavenプロジェクトをEclipseにインポートしたら、プロセス手順で使用できる最初のOSGiコンポーネントを書き込む開始の準備が整います。


### WorkflowProcessを実装するクラスを作成します

Eclipse IDEでMavenプロジェクトを開きます。 **projectname** > **core**フォルダーを展開します。 src/main/javaフォルダーを展開します。 「core」で終わるパッケージが表示されます。 このパッケージにWorkflowProcessを実装するJavaクラスを作成します。 executeメソッドをオーバーライドする必要があります。 executeメソッドのシグネチャは次のとおりです。
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException
executeメソッドは、次の3つの変数にアクセスします

**WorkItem**:workItem変数は、ワークフローに関連するデータへのアクセスを提供します。パブリックAPIドキュメントは[こちらから入手できます。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:このworkflowSession変数を使用すると、ワークフローを制御できます。パブリックAPIドキュメントは、[こちら](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)で入手できます

**MetaDataMap**:ワークフローに関連付けられているすべてのメタデータ。プロセスステップに渡されるプロセス引数は、MetaDataMapオブジェクトを使用して使用できます。[API に関するドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

このチュートリアルでは、アダプティブフォームに追加された添付ファイルをAEMワークフローの一部としてファイルシステムに書き込みます。

この使用例を達成するために、次のJavaクラスが記述されています

このコードを見てみましょう。

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
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
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

1行目 — コンポーネントのプロパティを定義します。 process.labelプロパティは、次のスクリーンショットの1つに示すように、OSGiコンポーネントをプロセス手順に関連付けるときに表示される内容です。

13 ～ 15行目 — このOSGiコンポーネントに渡されるプロセス引数は、「,」区切り文字を使用して分割されます。 次に、attachmentPathとsaveToLocationの値を文字列配列から抽出します。

* attachmentPath - AEMワークフローを呼び出すようにアダプティブフォームの送信アクションを設定した場合に、アダプティブフォームで指定したのと同じ場所です。 これは、ワークフローのペイロードに対するAEMでの添付ファイルの保存を希望するフォルダーの名前です。

* saveToLocation - AEMサーバーのファイルシステム上で添付ファイルを保存する場所です。

これらの2つの値は、次のスクリーンショットに示すように、プロセスの引数として渡されます。

![ProcessStep](assets/implement-process-step.gif)


19行目：次に、attachmentFilePathを作成します。 添付ファイルのパスは

    /var/fd/ダッシュボード/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* 「添付ファイル」は、アダプティブフォームの送信オプションを設定したときに指定されたワークフローのペイロードに関連するフォルダーの名前です。

   ![submitoptions](assets/af-submit-options.gif)

24 ～ 26行目 — Get ResourceResolverを取得し、次にattachmentFilePathを指すリソースを取得します。

残りのコードでは、APIを使用してattachmentFilePathを指すドキュメントの子オブジェクトを繰り返し処理することで、リソースオブジェクトを作成します。 このドキュメントオブジェクトはAEM Formsに固有です。 次に、ドキュメントオブジェクトのcopyToFileメソッドを使用して、ドキュメントオブジェクトを保存します。

>[!NOTE]
>
>AEM Formsに固有のドキュメントオブジェクトを使用するので、mavenプロジェクトにaemfd-client-sdkの依存関係を含める必要があります。 グループIDはcom.adobe.aemfdで、アーティファクトIDはaemfd-client-sdkです。

#### 構築と導入

[説明に従ってバンドルを構築します。バンドルが展開され、アクティブな状態であることを確認します。](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[](http://localhost:4502/system/console/bundles)

ワークフローモデルを作成する. ワークフローモデルにプロセスステップをドラッグ&amp;ドロップします。 プロセス手順を「アダプティブフォームの添付ファイルをファイルシステムに保存」に関連付けます。

必要なプロセス引数をコンマで区切って指定します。 例えば、Attachments,c:\\scrappp\\. 最初の引数は、アダプティブフォーム添付ファイルがワークフローのペイロードを基準に保存される際のフォルダーです。 これは、アダプティブフォームの送信アクションを設定する際に指定した値と同じである必要があります。 2番目の引数は、添付ファイルを保存する場所です。

アダプティブフォームの作成を参照してください。 「添付ファイル」コンポーネントをフォームにドラッグ&amp;ドロップします。 前の手順で作成したワークフローを呼び出すように、フォームの送信アクションを設定します。 適切な添付ファイルパスを指定します。

設定を保存します。

フォームをプレビューする. 2追加つの添付ファイルを作成し、フォームを送信します。 添付ファイルは、ワークフロー内で指定した場所のファイルシステムに保存されます。

