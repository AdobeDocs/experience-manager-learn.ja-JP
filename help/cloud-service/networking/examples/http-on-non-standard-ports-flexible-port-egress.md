---
title: 非標準ポートでの HTTP/HTTPS 接続（柔軟なポート出力用）
description: 柔軟なポートエグレス用の非標準ポートで動作するAEMから外部 Web サービスに、HTTP/HTTPS リクエストをas a Cloud Service的にする方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 非標準ポートでの HTTP/HTTPS 接続（柔軟なポート出力用）

非標準ポート (80/443ではなく ) での HTTP/HTTPS 接続は、AEM as a Cloud Serviceからプロキシ化する必要がありますが、特別な設定は必要ありません `portForwards` ルールを作成し、AEM Advanced Networking の `AEM_PROXY_HOST` および予約済みのプロキシポート `AEM_HTTP_PROXY_PORT` または `AEM_HTTPS_PROXY_PORT` の宛先は HTTP/HTTPS です。

## 高度なネットワークサポート

次のコード例は、次のアドバンスドネットワークオプションでサポートされています。

次を確認します。 [適切](../advanced-networking.md#advanced-networking) このチュートリアルに従う前に、高度なネットワーク設定が設定されています。

| 高度なネットワークがありません | [柔軟なポート出力](../flexible-port-egress.md) | [出力専用 IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> このコード例は、 [フレキシブルポートエグレス](../flexible-port-egress.md). 類似していますが、異なるコード例が [専用エグレス IP アドレスと VPN 用の非標準ポートでの HTTP/HTTPS 接続](./http-dedicated-egress-ip-vpn.md).

## コード例

この Java™コードの例は、AEM as a Cloud Serviceで実行できる OSGi サービスで、8080 上の外部 Web サーバーへの HTTP 接続をおこないます。 HTTPS Web サーバーへの接続では、環境変数を使用します `AEM_PROXY_HOST` および `AEM_HTTPS_PROXY_PORT` ( デフォルトは `proxy.tunnel:3128` (AEMリリース 6094 未満 )。

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
