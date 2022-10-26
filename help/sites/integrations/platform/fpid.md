---
title: AEMでのAdobe Experience Platform FPID の生成
description: AEMを使用してAdobe Experience Platform FPID Cookie を生成または更新する方法について説明します。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: aeeed85ec05de9538b78edee67db4d632cffaaab
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# AEMでのExperience PlatformFPID の生成

Adobe Experience Manager(AEM) とAdobe Experience Platform(AEP) の統合では、ユーザーアクティビティを一意に追跡するために、一意のファーストパーティデバイス ID(FPID)cookie を生成し、維持する必要があります。

サポートドキュメントを読む [ファーストパートのデバイス ID とExperience CloudID の連携の詳細を説明します](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

AEMを Web ホストとして使用する場合の FPID の仕組みの概要を以下に示します。

![AEMを使用した FPID と ECID](./assets/aem-platform-fpid-architecture.png)

## FPID を生成し、AEMで保持します。

AEM パブリッシュサービスは、CDN とAEM Dispatcher の両方のキャッシュで、要求をできるだけキャッシュすることで、パフォーマンスを最適化します。

一意性を保証するロジックを実装できる AEM Publish から直接提供されるのは、ユーザーごとの一意の FPID Cookie を生成し、FPID 値を返す HTTP 要求です。

FPID の一意性要件を組み合わせると、これらのリソースがキャッシュ不可能になるので、Web ページやその他のキャッシュ可能なリソースのリクエストで FPID Cookie を生成しないでください。

次の図に、AEM パブリッシュサービスによる FPID の管理方法を示します。

![FPID とAEMのフロー図](./assets/aem-fpid-flow.png)

1. Web ブラウザーは、AEMがホストする Web ページに対してリクエストをおこないます。 要求は、CDN またはAEM Dispatcher キャッシュからの Web ページのキャッシュされたコピーを使用して提供できます。
1. Web ページを CDN またはAEM Dispatcher キャッシュから提供できない場合、要求は AEM パブリッシュサービスに到達し、要求された Web ページが生成されます。
1. 次に、Web ページが Web ブラウザーに返され、要求に対応できなかったキャッシュが生成されます。 AEMの場合、CDN とAEM Dispatcher のキャッシュヒット率が 90%を超えることを想定します。
1. Web ページには、AEM パブリッシュサービス内のカスタム FPID サーブレットに対してキャッシュ不能な非同期 XHR(AJAX) リクエストを実行する JavaScript が含まれています。 これは（ランダムなクエリパラメーターと Cache-Control ヘッダーにより）キャッシュできないリクエストなので、CDN やAEM Dispatcher によってキャッシュされず、常に AEM パブリッシュサービスに到達して応答を生成します。
1. AEM パブリッシュサービスのカスタム FPID サーブレットがリクエストを処理し、既存の FPID Cookie が見つからない場合は新しい FPID を生成し、既存の FPID Cookie の有効期間を延長します。 また、このサーブレットは、クライアント側 JavaScript で使用する FPID を応答本文に返します。 幸いにも、カスタム FPID サーブレットロジックは軽量なので、このリクエストによって AEM パブリッシュサービスのパフォーマンスに影響を与えることはありません。
1. XHR リクエストの応答は、Platform Web SDK で使用するために、応答本文に FPID Cookie と FPID JSON を含むブラウザーに返されます。

## コードサンプル

次のコードと設定を AEM パブリッシュサービスにデプロイして、既存の FPID Cookie を生成または延長して FPID を JSON として返すエンドポイントを作成できます。

### AEM FPID Cookie サーブレット

FPID cookie を生成または拡張するには、 AEM HTTP エンドポイントを作成する必要があります ( [Sling サーブレット](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ このサーブレットは、 `/bin/aem/fpid` アクセスに認証は必要ないので、 認証が必要な場合は、Sling リソースタイプにバインドします。
+ このサーブレットは、HTTPGET要求を受け入れます。 応答が `Cache-Control: no-store` キャッシュを防ぐためにですが、一意のキャッシュバスティングクエリパラメーターを使用して、このエンドポイントをリクエストする必要があります。

HTTP リクエストがサーブレットに到達すると、サーブレットは、リクエストに FPID Cookie が存在するかどうかを確認します。

+ FPID cookie が存在する場合は、cookie の有効期間を延長し、その値を収集して応答に書き込みます。
+ FPID Cookie が存在しない場合は、新しい FPID Cookie を生成し、値を保存して応答に書き込みます。

次に、サーブレットが FPID をフォーム内の JSON オブジェクトとして応答に書き込みます。 `{ fpid: "<FPID VALUE>" }`.

FPID Cookie がマークされているので、本文でクライアントに FPID を提供することが重要です `HttpOnly`では、サーバーのみがその値を読み取れ、クライアント側の JavaScript ではその値を読み取れません。

応答本文の FPID 値は、Platform Web SDK を使用して呼び出しをパラメーター化するために使用されます。

以下に、AEMサーブレットエンドポイントのコード例を示します ( `HTTP GET /bin/aep/fpid`) を使用して FPID Cookie を生成または更新し、FPID を JSON として返します。

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### HTMLスクリプト

カスタムのクライアント側 JavaScript をページに追加して、サーブレットを非同期的に呼び出し、FPID Cookie を生成または更新して、応答で FPID を返す必要があります。

この JavaScript スクリプトは、次のいずれかのメソッドを使用して、通常、ページに追加されます。

+ [Adobe Experience Platformのタグ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM Client Library](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

カスタムAEM FPID サーブレットへの XHR 呼び出しは、非同期ですが高速なので、AEMが提供する Web ページにユーザーがアクセスし、リクエストが完了する前に移動することができます。
この場合、AEMから Web ページが次に読み込まれたときに、同じプロセスが再試行されます。

AEM FPID サーブレットへの HTTPGET(`/bin/aep/fpid`) は、ブラウザーと AEM パブリッシュサービスの間のインフラストラクチャが要求の応答をキャッシュしないように、ランダムクエリパラメーターでパラメーター化されます。
同様に、 `Cache-Control: no-store` キャッシュを避けるために、リクエストヘッダーが追加されました。

AEM FPID サーブレットの呼び出し時に、FPID が JSON 応答から取得され、 [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) を追加して、Experience PlatformAPI に送信します。

詳しくは、Experience Platformのドキュメントを参照してください。 [identityMap での FPID の使用](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Dispatcher 許可フィルター

最後に、カスタム FPID サーブレットへの HTTPGETリクエストは、AEM Dispatcher の `filter.any` 設定。

この Dispatcher 設定が正しく実装されていない場合、HTTPGETは `/bin/aep/fpid` 結果は 404 になります。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platformリソース

ファーストパーティExperience PlatformID(FPID) と Platform Web SDK を使用した ID データの管理について、次のデバイスドキュメントを確認します。

+ [ファーストパーティデバイス ID の生成](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Platform Web SDK のファーストパーティデバイス ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Platform Web SDK の ID データ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


