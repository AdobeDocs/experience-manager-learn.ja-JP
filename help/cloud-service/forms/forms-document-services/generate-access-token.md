---
title: アクセストークンの生成
description: Forms Document Services API を呼び出すためのアクセストークンを生成するサンプルコード
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 100cab4b-16bd-4358-b0f0-61dbfd64d412
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 100%

---

# アクセストークンの生成

アクセストークンは、REST API へのリクエストを認証および承認するために使用される資格情報です。通常、ユーザーまたはアプリケーションが正常にログインした後、認証サーバー（OAuth 2.0 または OpenID Connect など）によって発行されます。セキュリティ保護された Forms Document Services API を呼び出す際、クライアントの ID を確認するために、リクエストヘッダーにアクセストークンが含まれます。
アクセストークンの送信リクエストの例を以下に示します

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

アクセストークンの生成には、次のコードを使用しました

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

AccessTokenService クラスは、アドビの IMS 認証サービスから OAuth アクセストークンを取得する役割を果たします。JSON ファイル（server_credentials.json）からクライアント資格情報を読み取り、認証リクエストを作成して、Adobe トークンエンドポイントに送信します。その後、応答が解析されて、セキュア API 呼び出しに使用するアクセストークンが抽出されます。クラスには、信頼性を確保してデバッグをより簡単にする、適切なログとエラー処理が含まれています。

## 次の手順

[API 呼び出しの実行](./make-api-calls.md)
