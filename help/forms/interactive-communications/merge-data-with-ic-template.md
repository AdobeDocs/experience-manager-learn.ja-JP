---
title: データのマージによる印刷チャネルドキュメントの生成
seo-title: データのマージによる印刷チャネルドキュメントの生成
description: 入力ストリームに含まれるデータを結合して印刷チャネルドキュメントを生成する方法を学びます
seo-description: 入力ストリームに含まれるデータを結合して印刷チャネルドキュメントを生成する方法を学びます
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# 送信されたデータを使用した印刷チャネルドキュメントの生成

印刷チャネルドキュメントは、通常、フォームデータモデルのgetサービスを通じてバックエンドデータソースからデータをフェッチすることで生成されます。 場合によっては、提供されたデータを使用して印刷チャネルドキュメントを生成する必要があります。 例えば、顧客が受益者フォームの変更を入力し、送信済みフォームのデータを使用して印刷チャネルドキュメントを生成するとします。 この使用例を達成するには、次の手順を実行する必要があります

## 事前入力サービスの作成

このサービスにアクセスするには、サービス名「ccm-print-test」が使用されます。 この事前入力サービスが定義されたら、サーブレットまたはワークフロープロセス手順の実装でこのサービスにアクセスし、印刷チャネルドキュメントを生成できます。

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### WorkflowProcess実装の作成

workflowProcess実装コードスニペットを下に示します。このコードは、AEMワークフローのプロセスステップがこの実装に関連付けられている場合に実行されます。 この実装では、以下に説明する3つのプロセス引数が必要です。

* アダプティブフォームの設定時に指定されたDataFileパスの名前
* 印刷チャネルテンプレートの名前
* 生成された印刷チャネルドキュメントの名前

98行目 — アダプティブフォームはForm Data Modelに基づいているので、afBoundDataのdataノードに存在するデータが抽出されます。
128行目 — Data Optionsサービス名が設定されます。 サービス名をメモしておきます。 これは、前のコードリストの45行目で返された名前と一致する必要があります。
135行目 —ドキュメントは、PrintChannelオブジェクトのrenderメソッドを使用して生成されます。


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

サーバーでテストするには、次の手順に従います。

* [Day CQ Mailサービスを設定します。](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) これは、添付ファイルとして生成されたドキュメントを含む電子メールを送信するために必要です。
* [サービスユーザーバンドルを使用した開発の展開](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Apache Slingサービスユーザーマッパーサービス設定に次のエントリが追加されていることを確認します
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [この記事に関連するアセットをファイルシステムにダウンロードして解凍します。](assets/prefillservice.zip)
* [AEM package Managerを使用して次のパッケージを読み込みます](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [AEM Felix Web Consoleを使用して次をデプロイします](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar。 このバンドルには、この記事で言及されているコードが含まれています。

* [ChangeOfWenistierFormを開く](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* 次に示すように、アダプティブフォームがAEMワークフローに送信するように設定されていることを確認します
   ![画像](assets/generateic.PNG)
* [ワークフローモデルを設定します](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)プロセスステップと電子メールの送信コンポーネントが、ご使用の環境に応じて設定されていることを確認します
* [ChangeOfWenistierFormのプレビュー。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) 詳細を入力し、
* ワークフローが呼び出され、IC印刷チャネルドキュメントが、送信電子メールコンポーネントで指定された受信者に添付ファイルとして送信される必要があります
