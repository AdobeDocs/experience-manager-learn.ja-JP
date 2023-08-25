---
title: プライベート証明書を持つ内部 API を呼び出す
description: 非公開または自己署名の証明書を持つ内部 API を呼び出す方法を説明します。
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
kt: 11548
thumbnail: KT-11548.png
doc-type: article
last-substantial-update: 2023-08-25T00:00:00Z
source-git-commit: d4859d8af066d456f16f76869e99432aaa5b9863
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 0%

---


# プライベート証明書を持つ内部 API を呼び出す

プライベートまたは自己署名の証明書を使用して、AEMから Web API への HTTPS 呼び出しをおこなう方法を説明します。

デフォルトでは、自己署名証明書を使用する Web API への HTTPS 接続を試みると、次のエラーで接続が失敗します。

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

この問題は、通常、 **API の SSL 証明書は、認識された認証局 (CA) によって発行されません** Java™アプリケーションは SSL/TLS 証明書を検証できません。

を使用して、非公開または自己署名の証明書を持つ API を正常に呼び出す方法を学びます。 [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) および **AEM global TrustStore**.


## HttpClient を使用したプロトタイプの API 呼び出しコード

次のコードは、Web API への HTTPS 接続をおこないます。

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

コードでは、 [Apache HttpComponent](https://hc.apache.org/)&#39;s [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) ライブラリクラスとそのメソッド。


## HttpClient とAEM TrustStore の資料を読み込みます。

を持つ API エンドポイントを呼び出すには、以下を実行します。 _個人または自己署名の証明書_、 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)&#39;s `SSLContextBuilder` は、AEM TrustStore と共に読み込まれ、接続を容易にするために使用する必要があります。

次の手順に従います。

1. ログイン先 **AEM Author** as a **administrator**.
1. に移動します。 **AEM作成者/ツール/セキュリティ/Trust Store**&#x200B;をクリックし、 **グローバルトラストストア**. 初めてにアクセスする場合は、グローバルトラストストアのパスワードを設定します。

   ![グローバルトラストストア](assets/internal-api-call/global-trust-store.png)

1. プライベート証明書を読み込むには、 **証明書ファイルを選択** ボタンをクリックし、次を使用して目的の証明書ファイルを選択します。 `.cer` 拡張子。 「 」をクリックしてインポートします **送信** 」ボタンをクリックします。

1. 以下のように Java™コードを更新します。 を使用する場合は、 `@Reference` AEMを取得 `KeyStoreService` 呼び出しコードは、OSGi コンポーネント/サービス、または Sling モデル ( および `@OsgiService` が使用されています )。

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * OOTB を挿入する `com.adobe.granite.keystore.KeyStoreService` OSGi コンポーネントに OSGi サービスを組み込む。
   * 次を使用してグローバルAEM TrustStore を取得する `KeyStoreService` および `ResourceResolver`、 `getAEMTrustStore(...)` メソッドで実行できます。
   * 次のオブジェクトを作成： `SSLContextBuilder`(Java™を参照 ) [API の詳細](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * グローバルAEM TrustStore をに読み込む `SSLContextBuilder` using `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` メソッド。
   * 合格 `null` 対象： `TrustStrategy` 上記の方法では、API の実行中に、AEMの信頼された証明書のみが確実に成功します。


>[!CAUTION]
>
>CA が発行した有効な証明書を持つ API 呼び出しは、前述の方法で実行した場合、失敗します。 このメソッドに従う場合、AEMの信頼された証明書を持つ API 呼び出しのみが成功します。
>
>以下を使用します。 [標準的なアプローチ](#prototypical-api-invocation-code-using-httpclient) 有効な CA 発行証明書の API 呼び出しを実行する場合。つまり、前述の方法を使用して、プライベート証明書に関連付けられた API のみを実行する必要があります。

## JVM キーストアの変更を避ける

プライベート証明書を使用して内部 API を効果的に呼び出す従来の方法では、JVM キーストアの変更が必要です。 これは、Java™を使用してプライベート証明書を読み込むことで達成されます [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) コマンドを使用します。

ただし、この方法はセキュリティのベストプラクティスと一致しておらず、AEMは、 **グローバルトラストストア** および [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).
