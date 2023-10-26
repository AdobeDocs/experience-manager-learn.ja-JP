---
title: Azure ストレージへのフォーム送信の保存
description: REST API を使用した Azure ストレージへのフォームデータの保存
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 8%

---

# Azure ストレージからデータを取得

この記事では、Azure ストレージに保存されたデータをアダプティブフォームに入力する方法について説明します。
ここでは、アダプティブフォームのデータが Azure ストレージに保存されていることを前提としており、アダプティブフォームにそのデータを事前入力する必要があると想定しています。

## GETリクエストを作成

次の手順では、blobID を使用して Azure ストレージからデータを取得するコードを記述します。 データを取得するために次のコードが書き込まれました。 URL は、OSGi 設定の sasToken 値と storageURI 値、および getBlobData 関数に渡される blobID を使用して構築されました

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

        log.error("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.error("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

アダプティブフォームが `guid` パラメーターを URL 内で指定すると、テンプレートに関連付けられたカスタムページコンポーネントが取得され、Azure ストレージのデータを使用してアダプティブフォームに入力されます。
テンプレートに関連付けられているページコンポーネントには、次の JSP コードが含まれます。

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

* [カスタムアダプティブフォームテンプレートと、テンプレートに関連付けられたページコンポーネントを読み込みます。](./assets/store-and-fetch-from-azure.zip)

* [サンプルアダプティブフォームを読み込みます。](./assets/bank-account-sample-form.zip)

* OSGi 設定コンソールを使用して、Azure Portal Configuration で適切な値を指定します。
* [BankAccount フォームをプレビューして送信します](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* データが任意の Azure ストレージコンテナに保存されていることを確認します。 BLOB ID をコピーします。
* [BankAccount フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) Azure ストレージのデータが事前入力されるフォームの URL で、BLOB ID を guid パラメーターとして指定します。

