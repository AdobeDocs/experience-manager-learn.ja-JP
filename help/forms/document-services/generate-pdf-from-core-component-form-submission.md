---
title: コアコンポーネントベースのアダプティブフォームのデータを使用して PDF を生成する方法
description: ワークフローにおけるコアコンポーネントベースのフォーム送信のデータと XDP テンプレートの結合
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
exl-id: cae160f2-21a5-409c-942d-53061451b249
duration: 97
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '324'
ht-degree: 100%

---

# コアコンポーネントベースのフォーム送信のデータを使用して PDF を生成する方法

以下に示すのは、先頭を大文字にした「Core Components」を使用して改訂したテキストです。

典型的なシナリオでは、コアコンポーネントベースのアダプティブフォームを通じて送信されたデータから PDF を生成することが必要になります。このデータは常に JSON 形式です。Render PDF API を使用して PDF を生成するには、JSON データを XML 形式に変換する必要があります。`org.json.XML` の `toString` メソッドが、この変換に使用されます。詳しくは、`org.json.XML.toString` メソッドの[ドキュメント](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-)を参照してください。

## JSON スキーマに基づくアダプティブフォーム

次の手順に従って、アダプティブフォームの JSON スキーマを作成してください。

### XDP のサンプルデータを生成

プロセスを効率化するには、次の詳細な手順に従います。

1. XDP ファイルを AEM Forms Designer で開きます。
1. ファイル／フォームのプロパティ／プレビューに移動します。
1. 「プレビューデータを生成」を選択します。
1. 「生成」をクリックします。
1. `form-data.xml` などの意味のあるファイル名を割り当てます。

### XML データから JSON スキーマを生成

無料のオンラインツールを利用して、前の手順で生成した XML データを使用して [XML を JSON に変換](https://jsonformatter.org/xml-to-jsonschema)できます。

### JSON を XML に変換するカスタムワークフロープロセス

以下に示したコードでは、JSON を XML に変換し、結果の XML を `dataXml` という名前のワークフロープロセス変数に保存しています。

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### ワークフローの作成

フォームの送信を処理するには、次の 2 つの手順を含むワークフローを作成します。

1. 最初の手順では、カスタムプロセスを使用して、送信された JSON データを XML に変換します。
1. その後の手順では、XML データと XDP テンプレートを組み合わせて PDF を生成します。

![json-to-xml](assets/json-to-xml-process-step.png)


## サンプルコードのデプロイ

これをローカルサーバーでテストするには、次の効率化された手順に従います。

1. [AEM OSGi web コンソールを使用して、カスタムバンドルをダウンロードしインストールします](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar)。
1. [ワークフローパッケージを読み込みます](assets/workflow_to_render_pdf.zip)。
1. [サンプルのアダプティブフォームと XDP テンプレートを読み込みます](assets/adaptive_form_and_xdp_template.zip)。
1. [アダプティブフォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled)。
1. いくつかのフォームフィールドに入力します。
1. フォームを送信して AEM ワークフローを開始します。
1. レンダリングされた PDF をワークフローのペイロードフォルダーで見つけます。
