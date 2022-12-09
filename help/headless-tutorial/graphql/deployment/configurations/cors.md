---
title: AEM GraphQLの CORS 設定
description: AEM GraphQLで使用するためのクロスオリジンリソース共有 (CORS) の設定方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: 6f1000db880c3126a01fa0b74abdb39ffc38a227
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 3%

---


# クロスオリジンリソース共有 (CORS)

Adobe Experience Manager as a Cloud Serviceのクロスオリジンリソース共有 (CORS) は、AEM GraphQL API に対してブラウザーベースのクライアント側呼び出しをおこなうAEM以外の Web プロパティを容易にします。

>[!TIP]
>
> 次の設定は例です。 プロジェクトの要件に合わせて調整してください。

## CORS 要件

AEMに接続するクライアントがAEMと同じオリジン（ホストまたはドメインとも呼ばれます）から提供されない場合、AEM GraphQL API へのブラウザーベースの接続には、CORS が必要です。

| クライアントタイプ | [シングルページアプリ (SPA)](../spa.md) | [Web コンポーネント/JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 設定が必要 | ✔ | ✔ | ✘ | ✘ |

## OSGi 設定

AEM CORS OSGi 設定ファクトリは、CORS HTTP リクエストを受け入れるための許可条件を定義します。

| クライアントの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 設定が必要 | ✔ | ✔ | ✔ |


次の例は、AEM パブリッシュ (`../config.publish/..`) ですが、 [サポートされている実行モードフォルダー](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

主な設定プロパティは次のとおりです。

+ `alloworigin` および/または `alloworiginregexp` AEM Web に接続するクライアントの実行元を指定します。
+ `allowedpaths` 指定したオリジンから許可される URL パスパターンを指定します。
   + AEM GraphQLで永続化されたクエリをサポートするには、次のパターンを追加します。 `/graphql/execute.json.*`
   + エクスペリエンスフラグメントをサポートするには、次のパターンを追加します。 `/content/experience-fragments/.*`
+ `supportedmethods` は、CORS リクエストで許可される HTTP メソッドを指定します。 追加 `GET`を使用して、AEM GraphQLで保持されたクエリ（およびエクスペリエンスフラグメント）をサポートします。

[CORS OSGi の設定について詳しくは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=ja)

この設定例では、AEM GraphQLでの永続クエリの使用がサポートされています。 クライアント定義のGraphQLクエリを使用するには、 `allowedpaths` および `POST` から `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 承認済みのAEM GraphQL API リクエスト

認証が必要なAEM GraphQL API にアクセスする場合（通常は AEM オーサーまたは AEM パブリッシュ上の保護されたコンテンツ）、CORS OSGi 設定に次の追加の値が含まれていることを確認します。

+ `supportedheaders` リスト `"Authorization"`
+ `supportscredentials` が `true`

CORS 設定を必要とするAEM GraphQL API に対して許可されたリクエストは、通常、 [サーバー間アプリ](../server-to-server.md) したがって、CORS を設定する必要はありません。 CORS 設定が必要なブラウザーベースのアプリ（例： ） [シングルページアプリ](../spa.md) または [Web コンポーネント](../web-component.md)では通常、認証を使用します。資格情報を保護するのは困難です。

例えば、次の 2 つの設定は、 `CORSPolicyImpl` OSGi ファクトリ設定：

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### OSGi 設定の例

+ [OSGi 設定の例は、 WKND プロジェクトにあります。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 設定

AEM パブリッシュ（およびプレビュー）サービスの Dispatcher は、CORS をサポートするように設定する必要があります。

| クライアントの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher の CORS 設定が必要 | ✘ | ✔ | ✔ |

### HTTP リクエストでの CORS ヘッダーの許可

を更新します。 `clientheaders.any` HTTP リクエストヘッダーを許可するファイル `Origin`,  `Access-Control-Request-Method`、および `Access-Control-Request-Headers` をAEMに渡し、HTTP リクエストを [AEM CORS 設定](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher 設定の例

+ [Dispatcher の例 _クライアントヘッダー_ の設定は、WKND プロジェクトにあります。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### CORS HTTP 応答ヘッダーの配信

キャッシュする Dispatcher ファームの設定 **CORS HTTP 応答ヘッダー** AEM GraphQLで保持されたクエリが Dispatcher のキャッシュから提供される際に、それらが確実に含まれるようにするには、 `Access-Control-...` ヘッダーをキャッシュヘッダーリストに追加します。

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### Dispatcher 設定の例

+ [Dispatcher の例 _CORS HTTP 応答ヘッダー_ の設定は、WKND プロジェクトにあります。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
