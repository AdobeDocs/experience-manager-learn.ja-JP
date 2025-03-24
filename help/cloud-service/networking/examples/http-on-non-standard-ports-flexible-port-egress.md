---
title: フレキシブルポートエグレス用の非標準ポートでの HTTP／HTTPS 接続
description: AEM as a Cloud Service から、フレキシブルポートエグレス用の非標準ポートで動作する外部 web サービスに対して、HTTP／HTTPS リクエストを行う方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 86
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 100%

---

# フレキシブルポートエグレス用の非標準ポートでの HTTP／HTTPS 接続

非標準ポート（80／443ではない）での HTTP／HTTPS 接続は、AEM as a Cloud Service からプロキシ化する必要がありますが、特別な `portForwards` ルールは必要なく、AEM の高度なネットワーク機能の `AEM_PROXY_HOST` と予約済みのプロキシポート `AEM_HTTP_PROXY_PORT` または `AEM_HTTPS_PROXY_PORT`（宛先が HTTP／HTTPS のどちらであるかに応じて選択）を使用することができます。

## 高度なネットワーク機能のサポート

次のコードサンプルは、以下の高度なネットワーク機能オプションでサポートされています。

このチュートリアルを開始する前に、高度なネットワーク機能の設定が[適切](../advanced-networking.md#advanced-networking)にセットアップされていることを確認してください。

| 高度なネットワーク機能なし | [柔軟なポートエグレス](../flexible-port-egress.md) | [専用エグレス IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> このコードサンプルは、 [フレキシブルポートエグレス](../flexible-port-egress.md)のみを対象としています。異なるものの類似しているコードサンプルが、[専用エグレス IP アドレスと VPN 用の非標準ポートでの HTTP／HTTPS 接続](./http-dedicated-egress-ip-vpn.md)で利用可能です。

## コードサンプル

この Java™ コードサンプルは、AEM as a Cloud Service で動作する OSGi サービスで、8080 上の外部 web サーバーへの HTTP 接続を行います。HTTPS web サーバーへの接続では、環境変数 `AEM_PROXY_HOST` および `AEM_HTTPS_PROXY_PORT`（リリース 6094 より前の AEM では `proxy.tunnel:3128` がデフォルト）が使用されます。

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
