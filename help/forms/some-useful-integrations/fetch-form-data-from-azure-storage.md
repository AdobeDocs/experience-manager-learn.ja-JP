---
title: Azure ストレージへのフォーム送信の保存
description: REST API を使用した Azure ストレージへのフォームデータの保存
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 81
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 75%

---

# Azure ストレージからデータを取得

この記事では、Azure ストレージに保存されているデータをアダプティブフォームに入力する方法について説明します。
ここでは、アダプティブフォームのデータが Azure ストレージに保存されており、アダプティブフォームにそのデータを事前入力する必要があると想定しています。

## GET リクエストを作成

次の手順では、blobID を使用して Azure ストレージからデータを取得するコードを記述します。データを取得するには、次のコードを書き込みます。URL は、OSGi 設定の sasToken 値と storageURI 値、および getBlobData 関数に渡された blobID を使用して構築されました

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

URL に guid パラメーターが指定されたアダプティブフォームがレンダリングされると、テンプレートに関連付けられたカスタムページコンポーネントは、Azure ストレージからアダプティブフォームを取得し、そのデータを入力します。
次に、テンプレートに関連付けられたページコンポーネントの jsp 内のコードを示します。

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## ソリューションをテスト

* [カスタム OSGi バンドルのデプロイ](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [カスタムアダプティブフォームテンプレートと、テンプレートに関連付けられたページコンポーネントを読み込みます](./assets/store-and-fetch-from-azure.zip)

* [サンプルアダプティブフォームを読み込みます。](./assets/bank-account-sample-form.zip)

* OSGi 設定コンソールを使用して、Azure Portal の設定で適切な値を指定します。

* [BankAccount フォームをプレビューして送信](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* データが任意の Azure ストレージコンテナに保存されていることを確認します。Blob ID をコピーします。

* [BankAccount フォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6)し、Azure ストレージからのデータが事前に入力されるフォームの URL で、guid パラメーターとして Blob ID を指定します
