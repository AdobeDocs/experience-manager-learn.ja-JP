---
title: AEM でのクロスオリジンリソース共有（CORS）について
description: Adobe Experience Manager のクロスオリジンリソース共有（CORS）を使用すると、AEM 以外の Web プロパティで、認証済みおよび未認証の両方の AEM に対してクライアントサイドの呼び出しを行い、コンテンツをフェッチしたり、AEM と直接やり取りしたりできます。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 296
source-git-commit: e4c50b66f15b129197f22e104cedf698a1dac079
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# クロスオリジンリソース共有（[!DNL CORS]）について

Adobe Experience Manager のクロスオリジンリソース共有（[!DNL CORS]）を使用すると、AEM 以外の web プロパティを使用して、認証済みおよび未認証の両方で AEM へのクライアントサイド呼び出しを行い、コンテンツを取得したり、AEM と直接やり取りしたりできます。

このドキュメントで説明している OSGi 設定は、次の場合に十分です。

1. AEM パブリッシュでの単一オリジンリソース共有
2. AEM オーサーへの CORS アクセス

AEM パブリッシュでのマルチオリジン CORS アクセスが必要な場合は、[このドキュメント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ja#dispatcher-configuration)を参照してください。

## Adobe Granite クロスオリジンリソース共有ポリシー OSGi 設定

CORS 設定は、AEM で OSGi 設定ファクトリとして管理され、各ポリシーはファクトリの 1 つのインスタンスとして表されます。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite クロスオリジンリソース共有ポリシー OSGi 設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy]（`com.adobe.granite.cors.impl.CORSPolicyImpl`）

### ポリシーの選択

ポリシーは、

* `Allowed Origin` と `Origin` リクエストヘッダー
* および `Allowed Paths` とリクエストパスに比較することで選択されます。

これらの値に一致する最初のポリシーが使用されます。 何も見つからない場合、[!DNL CORS] リクエストが却下されます。

ポリシーがまったく設定されていない場合、サーバーの他のモジュールが [!DNL CORS] に応答しない限り、ハンドラーが無効になり、事実上拒否されるため、[!DNL CORS] リクエストも応答されません。

### Policy プロパティ

#### [!UICONTROL 許可された接触チャネル]

* `"alloworigin" <origin> | *`
* リソースにアクセスできる URI を指定する `origin` パラメーターのリスト。リクエストに資格情報がない場合、サーバーは &#42; をワイルドカードとして使用することで、任意の接触チャネルがリソースにアクセスできるようになります。 *実稼動環境で `Allow-Origin: *` を使用することは絶対にお勧めできません。これを使用すると、すべての外国（つまり、攻撃者）の web サイトに対して、CORS のないリクエストを実行を許可してしまいます。*

#### [!UICONTROL 許可されたオリジン（正規表現）]

* `"alloworiginregexp" <regexp>`
* リソースにアクセスできる URI を指定する `regexp` 正規表現のリスト。*正規表現は、慎重に構築されない場合、意図しない一致を引き起こす可能性があり、攻撃者は、ポリシーにも一致するカスタムドメイン名を使用できます。* `alloworigin` を使用して、特定の接触チャネルホスト名ごとに個別のポリシーを設定することをお勧めします。これが他のポリシー プロパティの設定を繰り返すことを意味する場合でも同様です。接触チャネルが異なると、ライフサイクルや要件が異なるので、明確に分離することに役立ちます。

#### [!UICONTROL 許可されているパス]

* `"allowedpaths" <regexp>`
* ポリシーが適用されるリソース パスを指定する `regexp` 正規表現のリスト。

#### [!UICONTROL 公開済みのヘッダー]

* `"exposedheaders" <header>`
* ブラウザーがアクセスできる応答ヘッダーを示すヘッダーパラメーターのリスト。CORS リクエスト（プリフライトではない）に対して、空でない場合、これらの値は `Access-Control-Expose-Headers` 応答ヘッダーにコピーされます。リスト内の値（ヘッダー名）は、ブラウザーからアクセス可能になります。この機能がないと、これらのヘッダーをブラウザーで読み取ることはできません。

#### [!UICONTROL 最大経過年数]

* `"maxage" <seconds>`
* プリフライトリクエストの結果をキャッシュできる期間を示す `seconds` パラメーター。

#### [!UICONTROL サポートされるヘッダー]

* `"supportedheaders" <header>`
* 実際のリクエストを行うときに使用できる HTTP リクエストヘッダーを示す `header` パラメーターのリスト。

#### [!UICONTROL 許可されるメソッド]

* `"supportedmethods"`
* 実際のリクエストを行う際に使用できる HTTP メソッドを示すメソッドパラメーターのリストです。

#### [!UICONTROL 資格情報をサポート]

* `"supportscredentials" <boolean>`
* リクエストに対する応答がブラウザーに公開できるかどうかを示す `boolean`。プリフライトリクエストへの応答として使用する場合、これは、資格情報を使用して実際のリクエストを実行できるかどうかを示します。

### 設定例

サイト 1 は、コンテンツが [!DNL GET] を介して消費される、匿名でアクセス可能な読み取り専用の基本的なシナリオです。

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ]
}
```

サイト 2 はより複雑で、承認および変更（POST、PUT、DELETE）リクエストが必要です。

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher のキャッシュに関する懸念と設定 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1 以降の応答ヘッダーはキャッシュ可能です。 これにより、リクエストが匿名である限り、[!DNL CORS] でリクエストされたリソースとともに [!DNL CORS] ヘッダーをキャッシュできます。

一般に、Dispatcher でのコンテンツのキャッシュに関する考慮事項と同じ点を、Dispatcher での CORS 応答ヘッダーのキャッシュに適用できます。 次の表は、[!DNL CORS] ヘッダー（[!DNL CORS] リクエスト）をキャッシュできるタイミングを定義します。

| キャッシュ可能 | 環境 | 認証ステータス | 説明 |
|-----------|-------------|-----------------------|-------------|
| いいえ | AEM パブリッシュ | 認証済み | AEM オーサー上の Dispatcher キャッシュは、オーサリングされていない静的アセットに限定されます。このため、HTTP 応答ヘッダーを含む AEM オーサー上のほとんどのリソースをキャッシュするのは困難であり、実用的ではありません。 |
| いいえ | AEM パブリッシュ | 認証済み | 認証済みリクエストでは CORS ヘッダーをキャッシュしないでください。リクエスト元ユーザーの認証／承認ステータスが配信済みリソースにどのような影響を与えるかを判断するのは困難なので、これは、認証済みリクエストをキャッシュしないという一般的なガイダンスに従っています。 |
| はい | AEM パブリッシュ | 匿名 | Dispatcher でキャッシュ可能な匿名リクエストは、応答ヘッダーもキャッシュできるので、今後の CORS リクエストで、キャッシュされたコンテンツにアクセスできるようになります。AEM パブリッシュで CORS 設定を変更した場合は、影響を受けるキャッシュ済みリソースを無効化する&#x200B;**必要があります** 。影響を受ける可能性のあるキャッシュ済みコンテンツを判断するのは難しいので、ベストプラクティスとしては、コードまたは設定のデプロイメントについては Dispatcher キャッシュをパージするように推奨されています。 |

### CORS リクエストヘッダーの許可

[必要な HTTP リクエストヘッダーを AEM に通過して処理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#specifying-the-http-headers-to-pass-through-clientheaders)できるようにするには、Disaptcher の `/clientheaders` 設定でこれらのヘッダーを許可する必要があります。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS 応答ヘッダーのキャッシュ

キャッシュされたコンテンツの CORS ヘッダーをキャッシュし提供できるようにするには、AEM パブリッシュの `dispatcher.any` ファイルに次の [/cache /headers configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#caching-http-response-headers) を追加します。

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

`dispatcher.any` ファイルを変更をしたら、**web サーバーアプリケーションを必ず再起動します**。

`/cache/headers` 設定の更新後、次のリクエストでヘッダーが適切にキャッシュされるように、キャッシュの完全なクリアが必要になる可能性があります。

## CORS のトラブルシューティング

ログは `com.adobe.granite.cors` で利用できます。

* `DEBUG` を有効にすると、[!DNL CORS] リクエストが拒否された理由の詳細が表示されます。
* `TRACE` を有効にすると、CORS ハンドラーを通じて実行されるすべてのリクエストの詳細が表示されます。

### ヒント：

* curl を使用して XHR リクエストを手動で再作成しますが、それぞれで違いが生じる可能性があるので、すべてのヘッダーと詳細を必ずコピーします。一部のブラウザーコンソールでは、curl コマンドをコピーできます。
* リクエストが、認証、CSRF トークンフィルター、Dispatcher フィルターまたはその他のセキュリティレイヤーではなく、CORS ハンドラーによって拒否されたかどうかを確認します。
   * CORS ハンドラーが 200 で応答したものの、応答に `Access-Control-Allow-Origin` ヘッダーがない場合、`com.adobe.granite.cors` の [!DNL DEBUG] で拒否のログを確認します。
* [!DNL CORS] リクエストの Dispatcher キャッシングが有効になっている場合
   * `/cache/headers` 設定が `dispatcher.any` に適用されており、web サーバーが正常に再起動されることを確認します。
   * OSGi または dispatcher.any の設定が変更された後で、キャッシュが正しくクリアされたことを確認します。
* 必要に応じて、リクエストに認証資格情報が存在することを確認します。

## サポート資料

* [クロスオリジンリソース共有ポリシーの AEM OSGi 設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [クロスオリジンリソース共有（W3C）](https://www.w3.org/TR/cors/)
* [HTTP アクセス制御（Mozilla MDN）](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Access_control_CORS)
