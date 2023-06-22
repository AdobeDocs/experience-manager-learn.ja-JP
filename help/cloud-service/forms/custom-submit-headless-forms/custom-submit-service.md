---
title: ヘッドレスアダプティブフォーム送信を処理するカスタム送信サービスを作成する
description: 送信されたデータに基づいてカスタム応答を返す
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 7%

---


# カスタム送信の作成

AEM Formsには、ほとんどの使用例を満たす多数の追加設定済み送信オプションが用意されています。これらの事前定義済み送信アクションに加えて、AEM Formsでは、必要に応じてフォーム送信を処理する独自のカスタム送信ハンドラーを作成できます。

カスタム送信サービスを作成するには、次の手順に従いました

## AEM Project を作成

既に既存のAEM FormsCloud Serviceプロジェクトがある場合は、 [カスタム送信サービスの作成に進む](#Write-the-custom-submit-service)

* c ドライブに cloudmanager というフォルダーを作成します。
* この新しく作成されたフォルダーに移動します
* の内容をコピーして貼り付けます。 [このテキストファイル](./assets/creating-maven-project.txt) コマンドプロンプトウィンドウで、DarchetypeVersion=41 を [最新バージョン](https://github.com/adobe/aem-project-archetype/releases). この記事の作成時点での最新バージョンは 41 でした。
* Enter キーを押してコマンドを実行します。すべてが正しく行われた場合は、ビルド成功のメッセージが表示されます。

## カスタム送信サービスの書き込み{#Write-the-custom-submit-service}

IntelliJ を起動し、AEMプロジェクトを開きます。 という新しい Java クラスを作成します。 **HandleRegistrationFormSubmission** 下のスクリーンショットに示すように
![custom-submit-service](./assets/custom-submit-service.png)

このサービスを実装するために、次のコードが記述されました。

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## アプリの下に crx ノードを作成します。

ui.apps ノードを展開し、という名前の新しいパッケージを作成します。 **HandleRegistrationFormSubmission** 次のスクリーンショットに示すように、apps ノードの下に
![crx-node](./assets/crx-node.png)
.content.xml という名前のファイルを **HandleRegistrationFormSubmission**. 次のコードを.content.xml にコピー&amp;ペーストします。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

の値 **submitService** 要素は一致する必要があります  **serviceName = &quot;Core Custom AF Submit&quot;** FormSubmitActionService 実装の

## ローカルのAEM Formsインスタンスにコードをデプロイします

変更を Cloud Manager リポジトリーにプッシュする前に、コードをローカルの Cloud Ready オーサーインスタンスにデプロイして、コードをテストすることをお勧めします。 オーサーインスタンスが実行中であることを確認します。
クラウド対応のオーサーインスタンスにコードをデプロイするには、AEMプロジェクトのルートフォルダーに移動し、次のコマンドを実行します

```
mvn clean install -PautoInstallSinglePackage
```

これにより、コードが 1 つのパッケージとしてオーサーインスタンスにデプロイされます

## コードを cloud manager にプッシュし、コードをデプロイします。

ローカルインスタンス上のコードを確認したら、コードをクラウドインスタンスにプッシュします。
変更をローカルの Git リポジトリにプッシュしてから、Cloud Manager リポジトリにプッシュします。 以下を参照してください。  [Git の設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html), [AEMプロジェクトの cloud manager リポジトリへのプッシュ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) および [開発環境へのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html) 記事。

パイプラインが正常に実行されたら、次のスクリーンショットに示すように、フォームの送信アクションをカスタム送信ハンドラーに関連付けることができます
![submit-action](./assets/configure-submit-action.png)

## 次の手順

[React アプリでカスタム応答を表示する](./handle-response-react-app.md)












