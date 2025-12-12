---
title: 顧客が管理する CDN を使用したカスタムドメイン名
description: 顧客が管理する CDN を使用する AEM as a Cloud Service web サイトにカスタムドメイン名を実装する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 100%

---

# 顧客が管理する CDN を使用したカスタムドメイン名

**顧客が管理する CDN** を使用する AEM as a Cloud Service web サイトにカスタムドメイン名を追加する方法について説明します。

このチュートリアルでは、顧客が管理する CDN を使用する Transport Layer Security（TLS）を使用して HTTPS アドレス可能なカスタムドメイン名 `wkndviaawscdn.enablementadobe.com` を追加して、サンプル [AEM WKND](https://github.com/adobe/aem-guides-wknd) サイトのブランディングを強化します。このチュートリアルでは、AWS CloudFront は顧客が管理する CDN として使用されますが、どの CDN プロバイダーも AEM as a Cloud Service との互換性を有する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

大まかな手順は次のとおりです。

![顧客が管理する CDN を使用したカスタムドメイン名](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## 前提条件

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) と [dig](https://www.isc.org/blogs/dns-checker/) が、ローカルマシンにインストールされている。
- 次のサードパーティのサービスへのアクセス権が付与されている。
   - 認証局（CA）- [DigitCert](https://www.digicert.com/) などのサイトドメインンに対して署名付き証明書をリクエストする場合
   - 顧客 CDN - AWS CloudFront、Azure CDN、Akamai などの顧客 CDN を設定し、SSL 証明書とドメインの詳細を追加する場合。
   - ドメイン名システム（DNS）ホスティングサービス - Azure DNS や AWS Route 53 などのカスタムドメインに対して DNS レコードを追加する場合。
- HTTP ヘッダー検証 CDN ルールを AEM as a Cloud Service 環境にデプロイする [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) へのアクセス権が付与されている。
- サンプル [AEM WKND](https://github.com/adobe/aem-guides-wknd) サイトを、[実稼動プログラム](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs)タイプの AEM as a Cloud Service 環境にデプロイする。

サードパーティのサービスへのアクセス権が付与されていない場合は、_セキュリティチームまたはホスティングチームと共同作業して手順を完了します_。

## SSL 証明書の生成

>[!VIDEO](https://video.tv.adobe.com/v/3441468?captions=jpn&quality=12&learn=on)

以下 2 つのオプションがあります。

1. `openssl` コマンドラインツールを使用 - サイトドメインの秘密鍵と証明書署名要求（CSR）を生成できます。署名付き証明書をリクエストするには、CSR を認証局（CA）に送信します。
1. ホスティングチームは、サイトに必要な秘密鍵と署名付き証明書を提供します。

最初のオプションの手順を確認してみましょう。

秘密鍵と CSR を生成するには、次のコマンドを実行し、プロンプトが表示されたら必要な情報を入力します。

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

署名付き証明書をリクエストするには、CA のドキュメントに従って、生成した CSR を CA に提供します。CA が CSR に署名すると、署名付き証明書ファイルを受け取ることができます。

### 署名付き証明書の確認

署名付き証明書を Cloud Manager に追加する前に確認することをお勧めします。次のコマンドを使用して証明書の詳細を確認できます。

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

署名付き証明書には、ルート証明書および中間証明書と、エンドエンティティ証明書を含む証明書チェーンが含まれる場合があります。

Adobe Cloud Manager では、エンドエンティティ証明書と証明書チェーンを&#x200B;_別のフォームフィールドで_&#x200B;受け入れるので、署名付き証明書からエンドエンティティ証明書と証明書チェーンを抽出する必要があります。

このチュートリアルでは、`*.enablementadobe.com` ドメインに対して発行された [DigitCert](https://www.digicert.com/) 署名付き証明書が例として使用されます。エンドエンティティと証明書チェーンは、署名付き証明書をテキストエディターで開き、`-----BEGIN CERTIFICATE-----` マーカーと `-----END CERTIFICATE-----` マーカーの間の内容をコピーすれば抽出されます。

## 顧客が管理する CDN の設定

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

AWS CloudFront、Azure CDN、Akamai などの顧客 CDN を設定し、SSL 証明書とドメインの詳細を追加します。このチュートリアルでは、例として AWS CloudFront を使用します。ただし、CDN ベンダーによっては、手順が異なる場合があります。主な注意事項は次のとおりです。

- SSL 証明書を CDN に追加します。
- カスタムドメイン名を CDN に追加します。
- 画像、CSS、JavaScript ファイルなどのコンテンツをキャッシュするように CDN を設定します。
- `X-Forwarded-Host` HTTP ヘッダーを CDN 設定に追加して、CDN が AEMCD 接触チャネルに送信するすべてのリクエストにこのヘッダーを含めるようにします。
- `Host` ヘッダーの値が、プログラム ID と環境 ID を含み、`adobeaemcloud.com` で終わるデフォルトの AEM as a Cloud Service ドメインに設定されていることを確認します。顧客 CDN から Adobe CDN に渡される HTTP ホストヘッダー値は、デフォルトの AEM as a Cloud Service ドメインにする必要があります。その他の値を指定すると、エラー状態になります。

## DNS レコードの設定

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

カスタムドメインの DNS レコードを設定するには、次の手順に従います。

1. CDN ドメイン名を指すカスタムドメインの CNAME レコードを追加します。

このチュートリアルでは、カスタムドメイン `wkndviaawscdn.enablementadobe.com` の CNAME レコードを Azure DNS に追加し、AWS CloudFront 配布ドメイン名を指すようにします。

### サイトの検証

カスタムドメイン名を使用してサイトにアクセスし、カスタムドメイン名を検証します。
AEM as a Cloud Service 環境の vhhost 設定によっては、機能する場合と機能しない場合があります。

重要なセキュリティ手順は、HTTP ヘッダー検証 CDN ルールを AEM as a Cloud Service 環境にデプロイすることです。ルールにより、リクエストが顧客 CDN からのものであり、他のソースからのものではないことが保証されます。

## HTTP ヘッダー検証 CDN ルールなしの現在の作業状態

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

HTTP ヘッダー検証 CDN ルールがない場合、`Host` ヘッダー値は、プログラム ID と環境 ID を含み、`adobeaemcloud.com` で終わるデフォルトの AEM as a Cloud Service ドメインに設定されます。Adobe CDN は、HTTP ヘッダー検証 CDN ルールがデプロイされている場合にのみ、`Host` ヘッダー値を顧客 CDN から受信した `X-Forwarded-Host` の値に変換します。それ以外の場合、`Host` ヘッダー値はそのまま AEM as a Cloud Service 環境に渡され、`X-Forwarded-Host` ヘッダーは使用されません。

### Host ヘッダー値を出力するサンプルサーブレットコード

次のサーブレットコードは、JSON 応答の `Host`、`X-Forwarded-*`、`Referer` および `Via` HTTP ヘッダー値を出力します。

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

サーブレットをテストするには、次の設定で `../dispatcher/src/conf.dispatcher.d/filters/filters.any` ファイルを更新します。また、CDN が `/bin/*` パスを&#x200B;**キャッシュしない**&#x200B;ように設定されていることも確認します。

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## HTTP ヘッダー検証 CDN ルールの設定とデプロイ

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

HTTP ヘッダー検証 CDN ルールを設定してデプロイするには、次の手順に従います。

- `cdn.yaml` ファイルに HTTP ヘッダー検証 CDN ルールを追加します。例を以下に示します。

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
            type: authenticate
            authenticator: edge-auth
  ```

- Cloud Manager UI を使用して、秘密鍵タイプの環境変数（CDN_EDGEKEY_080124、CDN_EDGEKEY_110124）を作成します。
- Cloud Manager パイプラインを使用して、HTTP ヘッダー検証 CDN ルールを AEM as a Cloud Service 環境にデプロイします。

## X-AEM-Edge-Key HTTPヘッダーで秘密鍵を渡す

>[!VIDEO](https://video.tv.adobe.com/v/3445046?captions=jpn&quality=12&learn=on)

顧客 CDN を更新して、`X-AEM-Edge-Key` HTTP ヘッダーで秘密鍵を渡すようにします。Adobe CDN では、この秘密鍵を使用して、リクエストが顧客 CDN からのものであることを検証し、`Host` ヘッダーの値を顧客 CDN から受信した`X-Forwarded-Host` の値に変換します。

## エンドツーエンドのビデオ

また、顧客が管理する CDN を使用して AEM as a Cloud Service でホストされるサイトにカスタムドメイン名を追加する上記の手順を示す、エンドツーエンドのビデオを視聴することもできます。

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
