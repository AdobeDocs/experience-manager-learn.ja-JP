---
title: HTML5フォームの送信の処理
description: HTML5フォーム送信ハンドラーの作成
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 4%

---


# HTML5フォームの送信の処理

HTML5フォームは、AEMでホストされるサーブレットに送信できます。 送信されたデータは、サーブレット内で入力ストリームとしてアクセスできます。 HTML5フォームを送信するには、AEM Forms Designerを使用してフォームテンプレートに「HTTP送信ボタン」を追加する必要があります

## 送信ハンドラーの作成

HTML5フォームの送信を処理するために、シンプルなサーブレットを作成できます。 送信されたデータは、次のコードを使用して抽出できます。 この[サーブレット](assets/html5-submit-handler.zip)は、このチュートリアルの一部として利用できます。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して[サーブレット](assets/html5-submit-handler.zip)をインストールしてください

9行目のコードを使用して、J2EEプロセスを呼び出すことができます。 コードを使用してJ2EEプロセスを呼び出す場合は、[AdobeLiveCycleクライアントSDK設定](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)を設定済みであることを確認してください。

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


## HTML5フォームの送信URLの設定

![submit-url](assets/submit-url.PNG)

* xdpをタップし、_プロパティ_->_詳細_&#x200B;をクリックします。
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.htmlをコピーし、「 URLを送信」テキストフィールドに貼り付けます。
* 「_保存して閉じる_」ボタンをクリックします。

### 除外パスへのエントリの追加

* [configMgr](http://localhost:4502/system/console/configMgr)に移動します。
* _AdobeGranite CSRF Filter_&#x200B;を検索します。
* 「Excluded Paths」セクションに次のエントリを追加します
* _/content/AemFormsSamples/handlehml5formsubmission_
* 変更を保存します

### フォームのテスト

* xdpテンプレートをタップします。
* 「_プレビュー_->HTMLとしてプレビュー
* フォームにデータを入力し、「送信」をクリックします
* サーバーのstdout.logファイルに書き込まれた送信済みデータが表示されます

### 追加情報

HTML5フォームの送信からPDFを生成する際には、この[記事](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)もお勧めします。




