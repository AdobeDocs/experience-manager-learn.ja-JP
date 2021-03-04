---
title: AEM Formsとマーケット（パート2）
seo-title: AEM Formsとマーケット（パート2）
description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 2%

---


# Marketo Authentication Service

MarketoのREST APIは、2本足のOAuth 2.0で認証されます。Marketoに対して認証を行うには、カスタム認証を作成する必要があります。 このカスタム認証は、通常、OSGIバンドル内に書き込まれます。 次のコードは、このチュートリアルの一部として使用されたカスタム認証子を示しています。

## カスタム認証サービス

次のコードは、Marketorに対する認証に必要なaccess_tokenを持つAuthenticationDetailsオブジェクトを作成します

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationServiceは、IAuthenticationインターフェイスを実装します。 このインターフェイスは、AEM FormsクライアントSDKの一部です。 サービスは、アクセストークンを取得し、AuthenticationDetailsのHttpHeaderにトークンを挿入します。 AuthenticationDetailsオブジェクトのHttpHeadersが設定されると、AuthenticationDetailsオブジェクトはフォームデータモデルの真皮層に戻されます。

getAuthenticationTypeメソッドが返す文字列に注意してください。 この文字列は、データソースを設定する際に使用します。

### アクセストークンの取得

単純なインターフェイスは、access_tokenを返す1つのメソッドで定義されます。 このインターフェイスを実装するクラスのコードは、ページの下の方に一覧表示されます。

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

次のコードは、REST API呼び出しを行う際に使用されるaccess_tokenを返すサービスのコードです。 このサービスのコードは、GET呼び出しを行うために必要な設定パラメーターにアクセスします。 このように、GETURLにclient_id,client_secretを渡してaccess_tokenを生成します。 次に、このaccess_tokenが呼び出し元のアプリケーションに返されます。

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

次のスクリーンショットに、設定する必要がある設定プロパティを示します。 これらの設定プロパティは、前述のコードで読み取り、access_tokenを取得します

![config](assets/marketoconfig.jfif)

### 設定

次のコードは、設定プロパティの作成に使用されました。 これらのプロパティは、Marketorインスタンスに固有です

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

次のコードは、設定プロパティを読み取り、getterメソッドを介して同じ値を返します

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. バンドルを構築し、AEMサーバーにデプロイします。
1. [ブラウザーで、「Marketo Credentials Service Configuration」のconfigMgrand検索を指](http://localhost:4502/system/console/configMgr) 定します。
1. Marketorインスタンスに固有の適切なプロパティを指定します
