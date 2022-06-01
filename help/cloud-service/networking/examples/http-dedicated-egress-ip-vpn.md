---
title: 専用の出力 IP アドレスと VPN 用の HTTP/HTTPS 接続
description: Dedicated Egress IP アドレスと VPN を実行するAEMから外部 Web サービスに HTTP/HTTPS リクエストをas a Cloud Serviceする方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# 専用の出力 IP アドレスと VPN 用の HTTP/HTTPS 接続

HTTP/HTTPS 接続は、AEM as a Cloud Serviceからプロキシ化する必要がありますが、特別な接続は必要ありません `portForwards` ルールを作成し、AEM Advanced Networking の `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`、および `AEM_HTTPS_PROXY_PORT`.

## 高度なネットワークサポート

次のコード例は、次のアドバンスドネットワークオプションでサポートされています。

次を確認します。 [適切](../advanced-networking.md#advanced-networking) このチュートリアルに従う前に、高度なネットワーク設定が設定されています。

| 高度なネットワークがありません | [柔軟なポート出力](../flexible-port-egress.md) | [出力専用 IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> このコード例は、 [出力専用 IP アドレス](../dedicated-egress-ip-address.md) および [VPN](../vpn.md). 類似していますが、異なるコード例が [フレキシブルポートエグレス用の非標準ポートでの HTTP/HTTPS 接続](./http-on-non-standard-ports-flexible-port-egress.md).

## コード例

この Java™コードの例は、AEM as a Cloud Serviceで実行できる OSGi サービスで、8080 上の外部 Web サーバーへの HTTP 接続をおこないます。 HTTPS Web サーバーへの接続では、 `AEM_HTTPS_PROXY_HOST` および `AEM_HTTPS_PROXY_PORT` の代わりに  `AEM_HTTP_PROXY_HOST` および `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> 次をお勧めします。 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) は、AEMからの HTTP/HTTPS 呼び出しに使用されます。

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // If the URL is http, use System.getenv("AEM_HTTP_PROXY_HOST") and System.getenv("AEM_HTTP_PROXY_PORT")
        // Else if the URL is https, us System.getenv("AEM_HTTPS_PROXY_HOST") and System.getenv("AEM_HTTPS_PROXY_PORT")

        if (System.getenv("AEM_HTTP_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_HTTP_PROXY_HOST and AEM_HTTP_PROXY_PORT
            ProxySelector proxySelector = ProxySelector.of(
                    new InetSocketAddress(System.getenv("AEM_HTTP_PROXY_HOST"),
                            Integer.parseInt(System.getenv("AEM_HTTP_PROXY_PORT"))));
            // Create an HttpClient and provide the proxy selector that will use AEM's native HTTP proxy configuration
            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_HTTP_PROXY");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_HTTP_PROXY");
        }

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
