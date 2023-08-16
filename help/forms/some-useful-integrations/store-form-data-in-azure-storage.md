---
title: Azure ストレージにフォーム送信を保存
description: REST API を使用して Azure ストレージにフォームデータを保存する
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
source-git-commit: 17f6148ce6f897052d9d13f23e3f1792646eb958
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Azure Storage にフォーム送信を保存する

この記事では、送信されたAEM Formsデータを Azure Storage に保存するための REST 呼び出しの実行方法を説明します。
送信されたフォームデータを Azure ストレージに保存するには、次の手順に従う必要があります。

## Azure ストレージアカウントの作成

[Azure ポータルアカウントにログインし、ストレージアカウントを作成します](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). ストレージアカウントにわかりやすい名前を付け、「Review」をクリックし、「Create」をクリックします。 これにより、すべてのデフォルト値を持つストレージアカウントが作成されます。 この記事の目的で、当社のストレージアカウントに名前を付けました。 `aemformstutorial`.

## 共有アクセスを作成

Azure Storage コンテナとのやり取りの認証の共有アクセス署名または SAS メソッドを使用します。
ポータルのストレージアカウントページで、左側の [ 共有アクセスの署名 ] メニュー項目をクリックし、新しい共有アクセスの署名キー設定ページを開きます。 次のスクリーンショットに示すように、設定と適切な終了日を必ず指定し、「 SAS と接続文字列を生成」ボタンをクリックします。 BLOB サービスの SAS URL をコピーします。 この URL を使用して HTTP 呼び出しをおこないます
![shared-access-keys](./assets/shared-access-signature.png)

## コンテナを作成

次に、フォーム送信のデータを格納するコンテナを作成する必要があります。
ストレージアカウントページで、左側の「コンテナ」メニュー項目をクリックし、「 」という名前のコンテナを作成します。 `formssubmissions`. パブリックアクセスレベルがプライベートに設定されていることを確認します。
![コンテナ](./assets/new-container.png)

## PUTリクエストを作成

次の手順では、送信されたフォームデータを Azure ストレージにPUTする保存リクエストを作成します。 BLOB サービスの SAS URL を変更して、URL にコンテナ名と BLOB ID を含める必要があります。 すべてのフォーム送信は、一意の BLOB ID で識別する必要があります。 一意の BLOB ID は、通常、コード内で作成され、PUT要求の URL に挿入されます。
次に、PUTリクエストの URL の一部を示します。 The `aemformstutorial` はストレージアカウントの名前で、formsubmissions は、データが一意の BLOB ID と共に保存されるコンテナです。 残りの URL は同じままです。
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

次の関数は、送信されたフォームデータを Azure ストレージに保存するために、PUTリクエストを使用して記述されます。 URL では、コンテナ名と uuid が使用されていることに注意してください。 以下に示すサンプルコードを使用して OSGi サービスまたは Sling サーブレットを作成し、フォーム送信を Azure ストレージに保存することができます。

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## コンテナに保存されたデータを検証

![form-data-in-container](./assets/form-data-in-container.png)





