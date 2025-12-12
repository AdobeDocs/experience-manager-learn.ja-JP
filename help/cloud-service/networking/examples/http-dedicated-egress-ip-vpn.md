---
title: 専用エグレス IP アドレスと VPN 用の HTTP／HTTPS 接続
description: AEM as a Cloud Service から、専用エグレス IP アドレスと VPN 用の外部 Web サービスに対して、HTTP／HTTPS リクエストを行う方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 70
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 100%

---

# 専用エグレス IP アドレスと VPN 用の HTTP／HTTPS 接続

HTTP/HTTPS 接続は、専用エグレス IP アドレスまたは VPN を使用して、AEM as a Cloud Service から自動的にプロキシ化されるので、特別な `portForwards` ルールは不要です。

## 高度なネットワーク機能のサポート

次のコードサンプルは、以下の高度なネットワーク機能オプションでサポートされています。

このチュートリアルを行う前に、[専用エグレス IP アドレスまたは VPN ](../advanced-networking.md#advanced-networking) の高度なネットワーキング機能が設定されていることを確認してください。

| 高度なネットワーク機能なし | [柔軟なポートエグレス](../flexible-port-egress.md) | [専用エグレス IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> このコード例は、[専用エグレス IP アドレス](../dedicated-egress-ip-address.md)および [VPN](../vpn.md) 用です。[フレキシブルポートエグレスの非標準ポートでの HTTP／HTTPS 接続](./http-on-non-standard-ports-flexible-port-egress.md)には、よく似た、異なるコードサンプルがあります。

## コードサンプル

この Java™ コードサンプルは、AEM as a Cloud Service で動作する OSGi サービスで、8080 上の外部 web サーバーへの HTTP 接続を行います。HTTPS（または HTTP）接続は、AEM as a Cloud Service から自動的にプロキシ化されるので、特別な開発は不要です。

>[!NOTE]
> AEM からの HTTP／HTTPS 呼び出しには [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) を使用することをお勧めします。

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
