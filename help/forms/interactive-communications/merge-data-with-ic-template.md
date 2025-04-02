---
title: データの結合による印刷チャネルドキュメントの生成
description: 入力ストリームに含まれるデータを結合して、印刷チャネルドキュメントを生成する方法を説明します
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 3bfbb4ef-0c51-445a-8d7b-43543a5fa191
last-substantial-update: 2019-07-07T00:00:00Z
duration: 151
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '445'
ht-degree: 100%

---

# 送信されたデータを使用した印刷チャネルドキュメントの生成

印刷チャネルのドキュメントは、通常、フォームデータモデルの get サービスを使用してバックエンドのデータソースからデータを取得して生成されます。 場合によっては、指定したデータを含む印刷チャネルドキュメントを生成する必要があります。 例えば、顧客が受取人フォームの変更に記入し、送信されたフォームのデータを使用して印刷チャネルドキュメントを生成するとします。 このユースケースを実現するには、次の手順に従う必要があります

## 事前入力サービスの作成

このサービスにアクセスするには、サービス名「ccm-print-test」を使用します。 この事前入力サービスを定義したら、サーブレットまたはワークフロープロセスステップの実装でこのサービスにアクセスすると、印刷チャネルドキュメントを生成できます。

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

### WorkflowProcess 実装の作成

以下に、workflowProcess 実装コードスニペットを示します。AEM Workflow のプロセスステップがこの実装に関連付けられている場合に、このコードを実行します。 この実装では、以下に示す 3 つのプロセス引数が必要です。

* アダプティブフォームの設定時に指定した DataFile パスの名前
* 印刷チャネルテンプレートの名前
* 生成された印刷チャネルドキュメントの名前

98 行目 - アダプティブフォームはフォームデータモデルに基づいているので、 afBoundData のデータノードに存在するデータが抽出されます。
128 行目 - データオプションサービス名が設定されています。サービス名をメモしておきます。 前のコードリストの 45 行目で返された名前と一致する必要があります。
135 行目 - PrintChannel オブジェクトのレンダーメソッドを使用してドキュメントが生成されます


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

ご使用のサーバーでこれをテストするには、次の手順に従ってください。

* [Day CQ メールサービスを設定します。](https://helpx.adobe.com/jp/experience-manager/6-5/communities/using/email.html)これは、生成されたドキュメントを添付ファイルとして含むメールを送信するために必要です。
* [サービスユーザーバンドルを使用した開発のデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Apache Sling Service User Mapper Service の設定に次のエントリが追加されていることを確認します。
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [この記事に関連するアセットをファイルシステムにダウンロードして解凍します。](assets/prefillservice.zip)
* [AEM パッケージマネージャーを使用して、次のパッケージを読み込みます。](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [AEM Felix web コンソールを使用して以下をデプロイします。](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. このバンドルには、この記事で紹介されているコードが含まれています。

* [ChangeOfBeneficiaryForm を開きます。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* 次に示すように、必ずアダプティブフォームが AEM のワークフローに送信されるように設定します。
  ![画像](assets/generateic.PNG)
* [ワークフローモデルを設定します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)プロセスステップとメール送信コンポーネントが、お使いの環境に応じて設定されていることを確認します。
* [ChangeOfBeneficiaryForm をプレビューします。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) 詳細情報を入力して送信します。
* ワークフローが起動し、送信メールコンポーネントで指定された受信者に、IC 印刷チャネルドキュメントが添付ファイルとして送信されます。
