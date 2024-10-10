---
title: HTML5 フォーム送信の処理
description: HTML5 フォーム送信ハンドラーの作成
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '251'
ht-degree: 100%

---

# HTML5 フォーム送信の処理

HTML5 フォームは、AEM でホストされるサーブレットに送信できます。送信されたデータは、サーブレットで入力ストリームとしてアクセスできます。HTML5 フォームを送信するには、AEM Forms Designer を使用してフォームテンプレートに「HTTP 送信ボタン」を追加する必要があります。

## 送信ハンドラーの作成

HTML5 フォーム送信を処理する簡単なサーブレットを作成できます。送信されたデータは、次のコードを使用して抽出できます。この[サーブレット](assets/html5-submit-handler.zip)は、このチュートリアルの一部として利用できます。[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して[サーブレット](assets/html5-submit-handler.zip)をインストールしてください。

9 行目以降のコードを使用して、J2EE プロセスを呼び出すことができます。このコードを使用して J2EE プロセスを呼び出す場合は、[Adobe LiveCycle Client SDK Configuration](https://helpx.adobe.com/jp/aem-forms/6/submit-form-data-livecycle-process.html) が設定済みであることを確認してください。

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


## HTML5 フォームの送信 URL の設定

![submit-url](assets/submit-url.PNG)

* xdp をタップし、_プロパティ_／_詳細_&#x200B;をクリックします。
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html をコピーし、「送信 URL」テキストフィールドに貼り付けます。
* 「_保存して閉じる_」ボタンをクリックします。

### 除外パスへのエントリの追加

* [configMgr](http://localhost:4502/system/console/configMgr) に移動します。
* _Adobe Granite CSRF フィルター_&#x200B;を検索します。
* 「除外されるパス」セクションに次のエントリを追加します。
* _/content/AemFormsSamples/handlehml5formsubmission_
* 変更内容を保存します。

### フォームのテスト

* xdp テンプレートをタップします。
* _プレビュー_／HTML としてプレビューをクリックします。
* フォームに何らかのデータを入力し、「送信」をクリックします。
* サーバーの stdout.log ファイルに書き込まれた送信済みデータが表示されます。

### 追加情報

HTML5 フォーム送信による PDF の生成に関するこの[記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=ja)もお勧めします。
