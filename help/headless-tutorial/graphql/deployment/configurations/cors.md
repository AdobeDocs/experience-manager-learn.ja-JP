---
title: AEM GraphQL の CORS 設定
description: AEM GraphQLで使用するためのクロスオリジンリソース共有（CORS）の設定方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: 36b4217a899b462296d4389bc96a644da97d5da4
workflow-type: ht
source-wordcount: '619'
ht-degree: 100%

---

# クロスオリジンリソース共有（CORS）

Adobe Experience Manager as a Cloud Service のクロスオリジンリソース共有（CORS）は、AEM 以外の web プロパティが、AEM の GraphQL API に対してブラウザーベースのクライアントサイド呼び出しを行うのを容易にします。

次の記事では、CORS を介して AEM ヘッドレス エンドポイントの特定のセットへの&#x200B;_単一オリジン_&#x200B;アクセスを設定する方法について説明します。単一オリジンとは、単一の非 AEM ドメインのみが AEM にアクセスすることを意味します。例えば、https://app.example.com が https://www.example.com に接続します。キャッシュに関する懸念があるので、マルチオリジンアクセスは、この方法を使用して動作しない可能性があります。

>[!TIP]
>
> 設定例は次のようになります。プロジェクトの要件に合わせて調整してください。

## CORS 要件

AEM に接続するクライアントが AEM と同じオリジン（ホストまたはドメインとも呼ばれる）から提供されない場合、AEM GraphQL API へのブラウザーベースの接続には、CORS が必要です。

| クライアントタイプ | [シングルページアプリ（SPA）](../spa.md) | [Web コンポーネント／JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 設定が必要 | ✔ | ✔ | ✘ | ✘ |

## OSGi 設定

AEM CORS OSGi 設定ファクトリは、CORS HTTP リクエストを受け入れるための許可条件を定義します。

| クライアントの接続先 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 設定が必要 | ✔ | ✔ | ✔ |


次の例は AEM パブリッシュ（`../config.publish/..`）ですが、 [任意のサポートされている実行モードフォルダー](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution?lang=ja)に追加できます。

主な設定プロパティは次のとおりです。

+ `alloworigin` や `alloworiginregexp` は AEM web に接続するクライアントの実行元を指定します。
+ `allowedpaths` は指定したオリジンから許可される URL パスパターンを指定します。
   + AEM GraphQLで永続化クエリをサポートするには、パターン「`/graphql/execute.json.*`」を追加します。
   + エクスペリエンスフラグメントをサポートするには、パターン「`/content/experience-fragments/.*`」を追加します。
+ `supportedmethods` は、CORS リクエストで許可される HTTP メソッドを指定します。 `GET` を追加 して、AEM GraphQLで保持されたクエリ（およびエクスペリエンスフラグメント）をサポートします。

[ CORS OSGi の設定について詳しくは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=ja)

この設定例では、AEM GraphQL での永続クエリの使用がサポートされています。 クライアント定義の GraphQL クエリを使用するには、`allowedpaths` および `POST` からの GraphQL エンドポイント URL を `supportedmethods` に追加します。

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
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
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 承認済みの AEM GraphQL API リクエスト

認証が必要な AEM GraphQL API にアクセスする場合（通常は AEM オーサーまたは AEM パブリッシュ上の保護されたコンテンツ）、CORS OSGi 設定に次の追加の値が含まれていることを確認します。

+ `supportedheaders` には `"Authorization"` もリストされています
+ `supportscredentials` は `true` に設定されています。

CORS 設定を必要とするAEM GraphQL API に対して許可されたリクエストは、通常、[サーバー間アプリ](../server-to-server.md) のコンテキストで発生するため、CORS 設定を必要としないため、異常です。CORS 設定が必要なブラウザーベースのアプリ（例：[シングルページアプリ](../spa.md) または [web コンポーネント](../web-component.md)）では通常、資格情報を保護するのが困難なため認証を使用します。

たとえば、これらの 2 つの設定は、`CORSPolicyImpl` OSGi ファクトリ設定では次のように設定されます。

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

+ [OSGi 設定の例は、WKND プロジェクトで確認できます。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 設定

AEM パブリッシュ（およびプレビュー）サービスの Dispatcher は、CORS をサポートするように設定する必要があります。

| クライアントの接続先 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher の CORS 設定が必要 | ✘ | ✔ | ✔ |

### HTTP リクエストでの CORS ヘッダーの許可

`clientheaders.any` ファイルを更新して、HTTP リクエストヘッダー `Origin`、`Access-Control-Request-Method`、`Access-Control-Request-Headers` が AEM に渡され、HTTP リクエストが[ AEM CORS 構成](#osgi-configuration)によって処理されるようにします。

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

+ [Dispatcher の例 _クライアントヘッダー_&#x200B;の設定は、WKND プロジェクトで確認できます。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


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

+ [Dispatcher の例 _CORS HTTP 応答ヘッダー_&#x200B;の設定は、WKND プロジェクトで確認できます。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
