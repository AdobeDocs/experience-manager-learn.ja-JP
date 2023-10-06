---
title: 相互トランスポート層セキュリティ (mTLS) 認証
description: 相互トランスポート層セキュリティ (mTLS) 認証が必要なAEMから Web API への HTTPS 呼び出しをおこなう方法について説明します。
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
kt: 13881
thumbnail: KT-13881.png
doc-type: article
last-substantial-update: 2023-10-10T00:00:00Z
source-git-commit: 2f0490263eaf5e3458e2d71113411a4fdd0aa94c
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 12%

---


# 相互トランスポート層セキュリティ (mTLS) 認証

相互トランスポート層セキュリティ (mTLS) 認証が必要なAEMから Web API への HTTPS 呼び出しをおこなう方法について説明します。

mTLS または双方向 TLS 認証により、 **クライアントとサーバの両方が互いに認証を行う**. この認証は、電子証明書を使用しておこなわれます。 これは、強力なセキュリティと ID の検証が重要なシナリオで一般的に使用されます。

デフォルトでは、mTLS 認証が必要な Web API への HTTPS 接続を試みると、次のエラーで接続が失敗します。

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

この問題は、クライアントが自身を認証する証明書を提示しない場合に発生します。

mTLS 認証を必要とする API を、 [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) および **AEMキーストアと TrustStore**.


## HttpClient とAEM KeyStore の資料を読み込む

mTLS 保護 API をAEMから呼び出すには、次の手順を大まかに実行する必要があります。

### AEM Certificate Generation

組織のセキュリティチームと提携して、AEM証明書をリクエストします。 セキュリティチームは、証明書に関する詳細 ( キー、証明書署名要求 (CSR)、CSR を使用して証明書が発行されるなど ) を提供または要求します。

デモ用に、証明書関連の詳細 ( キー、証明書署名要求 (CSR) など ) を生成します。 次の例では、自己署名 CA を使用して証明書を発行します。

- まず、内部証明機関 (CA) 証明書を生成します。

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- AEM証明書を生成します。

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- AEM秘密鍵を DER 形式に変換します。AEM KeyStore では、秘密鍵が DER 形式で必要です。

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>自己署名 CA 証明書は、開発目的でのみ使用されます。 実稼動環境では、信頼された認証局 (CA) を使用して証明書を発行します。


### 証明書交換

AEM証明書に自己署名 CA を使用している場合は、上記のように、証明書または内部の証明機関 (CA) 証明書を API プロバイダーと交換します。

また、API プロバイダーが自己署名 CA 証明書を使用している場合は、API プロバイダーから証明書または内部証明機関 (CA) 証明書を受け取ります。

### 証明書のインポート

AEM証明書を読み込むには、次の手順に従います。

1. にログインします。 **AEM Author** as a **administrator**.

1. に移動します。 **AEM作成者/ツール/セキュリティ/ユーザー/作成または既存のユーザーの選択**.

   ![既存のユーザーを作成または選択](assets/mutual-tls-authentication/create-or-select-user.png)

   デモ用に、という名前の新しいユーザーが `mtl-demo-user` が作成されました。

1. を開くには、 **ユーザープロパティ**」をクリックし、ユーザー名をクリックします。

1. クリック **キーストア** タブを押し、 **キーストアを作成** 」ボタンをクリックします。 次に、 **キーストアのアクセスパスワードを設定** ダイアログで、このユーザーのキーストアのパスワードを設定し、「保存」をクリックします。

   ![キーストアを作成](assets/mutual-tls-authentication/create-keystore.png)

1. 新しい画面で、 **DER ファイルから秘密鍵を追加** セクションでは、次の手順に従います。

   1. エイリアスを入力

   1. 上記で生成された DER 形式でAEM秘密鍵を読み込みます。

   1. 上記で生成された証明書チェーンファイルをインポートします。

   1. 送信をクリック

      ![AEM秘密鍵を読み込む](assets/mutual-tls-authentication/import-aem-private-key.png)

1. 証明書が正常に読み込まれたことを確認します。

   ![AEM秘密鍵と証明書が読み込まれました](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

API プロバイダーが自己署名 CA 証明書を使用している場合は、受け取った証明書をAEM TrustStore に読み込み、次の手順に従います。 [ここ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html#httpclient-and-load-aem-truststore-material).

同様に、AEMが自己署名 CA 証明書を使用している場合は、API プロバイダーに読み込むように要求します。

### HttpClient を使用した一般的な mTLS API 呼び出しコードの作成

Java™コードを以下のように更新します。次を使用するには： `@Reference` AEMを取得するための注釈 `KeyStoreService` service 呼び出し元のコードは、OSGi コンポーネントまたはサービス、または Sling モデル ( および `@OsgiService` が使用されています )。


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi サービスを OSGi コンポーネントに挿入します。
- 次を使用してユーザーのAEMキーストアを取得する `KeyStoreService` および `ResourceResolver`、 `getAEMKeyStore(...)` メソッドで実行できます。
- API プロバイダーが自己署名 CA 証明書を使用している場合は、グローバルAEM TrustStore ( `getAEMTrustStore(...)` メソッドで実行できます。
- `SSLContextBuilder` のオブジェクトを作成します。Java™ [API の詳細](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html)を参照してください。
- ユーザーのAEMキーストアをに読み込みます。 `SSLContextBuilder` using `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)` メソッド。
- キーストアのパスワードは、キーストアの作成時に設定されたパスワードです。OSGi 設定で保存する必要があります。詳しくは、 [シークレットの設定値](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values?lang=ja).

## JVM キーストアの変更の回避

非公開証明書を使用して mTLS API を効果的に呼び出す従来の方法では、JVM キーストアの変更が行われています。 これは、Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) コマンドを使用してプライベート証明書を読み込むことで実現します。

ただし、この方法はセキュリティのベストプラクティスと一致しておらず、AEMは、 **ユーザー固有のキーストアとグローバル TrustStore** および [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).