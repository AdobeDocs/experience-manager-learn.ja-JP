---
title: AEM GraphQL の CORS 設定
description: AEM GraphQL で使用するためのクロスオリジンリソース共有 (CORS) の設定方法を説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 4%

---


# クロスオリジンリソース共有 (CORS)

Adobe Experience Manager as a Cloud Serviceのクロスオリジンリソース共有 (CORS) は、AEM以外の Web プロパティを容易にして、AEM GraphQL API に対するブラウザーベースのクライアント側呼び出しをおこないます。

>[!TIP]
>
> 次の設定は例です。 プロジェクトの要件に合わせて調整してください。



## CORS 要件

AEMに接続するクライアントがAEMと同じ接触チャネル（ホストまたはドメインとも呼ばれます）から提供されない場合、AEM GraphQL API へのブラウザーベースの接続には CORS が必要です。

| クライアントタイプ | シングルページアプリ (SPA) | Web コンポーネント/JS | モバイル | サーバー間 |
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
+ `allowedpaths` 指定したオリジンから許可される URL パスパターンを指定します。 AEM GraphQL の永続クエリをサポートするには、次のパターンを使用します。 `"/graphql/execute.json.*"`
+ `supportedmethods` は、CORS リクエストで許可される HTTP メソッドを指定します。 追加 `GET`、 AEM GraphQL 永続クエリをサポートするために使用します。

[CORS OSGi の設定について詳しくは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=ja)


この設定例では、AEM GraphQL の永続クエリの使用がサポートされています。 クライアント定義の GraphQL クエリを使用するには、に GraphQL エンドポイント URL を追加します。 `allowedpaths` および `POST` から `supportedmethods`.

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
    "/graphql/execute.json.*"
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
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## Dispatcher 設定

AEM パブリッシュ（およびプレビュー）サービスの Dispatcher は、CORS をサポートするように設定する必要があります。

| クライアントの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher の CORS 設定が必要 | ✘ | ✔ | ✔ |

### HTTP リクエストでの CORS ヘッダーの許可

を更新します。 `clientheaders.any` HTTP リクエストヘッダーを許可するファイル `Origin`,  `Access-Control-Request-Method` および `Access-Control-Request-Headers` をAEMに渡し、HTTP リクエストを [AEM CORS 設定](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### CORS HTTP 応答ヘッダーの配信

キャッシュする Dispatcher ファームの設定 **CORS HTTP 応答ヘッダー** AEM GraphQL の永続クエリが Dispatcher キャッシュから提供される際に、それらを確実に含めるには、 `Access-Control-...` ヘッダーをキャッシュヘッダーリストに追加します。

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
