---
title: HTML5 フォーム送信を処理
description: HTML5 フォーム送信ハンドラーを作成
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 3%

---

# HTML5 フォーム送信を処理

HTML5 のフォームは、AEMでホストされるサーブレットに送信できます。 送信されたデータは、サーブレットで入力ストリームとしてアクセスできます。 HTML5 フォームを送信するには、AEM Forms Designer を使用してフォームテンプレートに「HTTP 送信ボタン」を追加する必要があります

## 送信ハンドラーを作成する

単純なサーブレットを作成して、HTML5 フォームの送信を処理できます。 送信されたデータは、次のコードを使用して抽出できます。 この [servlet](assets/html5-submit-handler.zip) は、このチュートリアルの一部として使用できます。 をインストールしてください [servlet](assets/html5-submit-handler.zip) using [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)

9 行目のコードを使用して、J2EE プロセスを呼び出すことができます。 が設定されていることを確認してください [AdobeLiveCycleクライアント SDK の設定](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) コードを使用して J2EE プロセスを呼び出す場合。

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## 送信フォームの送信 URL のHTML5

![submit-url](assets/submit-url.PNG)

* xdp をタップし、 _プロパティ_->_詳細_
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.htmlをコピーし、「 URL を送信」テキストフィールドに貼り付けます
* クリック _SaveAndClose_ 」ボタンをクリックします。

### Exclude パスにエントリを追加

* に移動します。 [configMgr](http://localhost:4502/system/console/configMgr).
* を検索 _AdobeGranite CSRF フィルタ_
* 「除外されたパス」セクションに次のエントリを追加します。
* _/content/AemFormsSamples/handlehml5formsubmission_
* 変更を保存します

### フォームをテストする

* xdp テンプレートをタップします。
* クリック _プレビュー_-> プレビューHTML
* フォームにデータを入力し、「送信」をクリックします
* サーバーの stdout.log ファイルに書き込まれた送信済みデータが表示されます

### 追加情報

この [記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) また、HTML5 からPDFを生成する際に、フォームの送信もお勧めします。
