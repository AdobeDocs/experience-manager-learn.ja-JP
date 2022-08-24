---
title: AEMを使用したクロスオリジンリソース共有 (CORS) について
description: Adobe Experience Managerのクロスオリジンリソース共有 (CORS) は、AEM以外の Web プロパティを容易にして、認証済みと未認証の両方で、AEMに対するクライアント側呼び出しをおこない、コンテンツを取得したり、AEMと直接やり取りします。
version: 6.4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 41be8c934bba16857d503398b5c7e327acd8d20b
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 1%

---

# クロスオリジンリソース共有 ([!DNL CORS])

Adobe Experience Managerのクロスオリジンリソース共有 ([!DNL CORS]) は、AEM以外の web プロパティを容易にして、AEMに対するクライアント側の呼び出し（認証済みと未認証の両方）でコンテンツを取得したり、AEMと直接やり取りしたりできます。

## AdobeGranite クロスオリジンリソース共有ポリシー OSGi 設定

CORS 設定は、AEMで OSGi 設定ファクトリとして管理され、各ポリシーはファクトリの 1 つのインスタンスとして表されます。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite クロスオリジンリソース共有ポリシー OSGi 設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### ポリシーの選択

ポリシーは、

* `Allowed Origin` と `Origin` リクエストヘッダー
* および `Allowed Paths` をリクエストパスに置き換えます。

これらの値に一致する最初のポリシーが使用されます。 何も見つからない場合、 [!DNL CORS] リクエストが拒否されます。

ポリシーが設定されていない場合、 [!DNL CORS] 要求に対して応答するサーバーの他のモジュールがない限り、ハンドラーが無効になり、結果的に拒否されるので、要求に対して応答しません。 [!DNL CORS].

### Policy プロパティ

#### [!UICONTROL 許可されたオリジン]

* `"alloworigin" <origin> | *`
* リスト `origin` リソースにアクセスできる URI を指定するパラメーター。 認証情報を持たない要求の場合、サーバーは &#42; をワイルドカードとして使用することで、任意の接触チャネルがリソースにアクセスできるようになります。 *次を使用することは絶対にお勧めしません。 `Allow-Origin: *` 実稼動環境では、CORS を使用しないすべての外部（攻撃者）web サイトに対し、ブラウザーで厳密に禁止される要求を実行できるので、*

#### [!UICONTROL 許可された起源（正規表現）]

* `"alloworiginregexp" <regexp>`
* リスト `regexp` リソースにアクセスできる URI を指定する正規表現。 *正規表現は、慎重に構築されない場合、意図しない一致を引き起こす可能性があり、攻撃者は、ポリシーにも一致するカスタムドメイン名を使用できます。* 通常は、特定のオリジンホスト名ごとに個別のポリシーを設定し、 `alloworigin`（他のポリシープロパティの設定を繰り返し行う場合でも） 起源が異なると、ライフサイクルや要件が異なるので、明確に分離することに役立ちます。

#### [!UICONTROL 許可されているパス]

* `"allowedpaths" <regexp>`
* リスト `regexp` ポリシーを適用するリソースパスを指定する正規表現。

#### [!UICONTROL 公開済みのヘッダー]

* `"exposedheaders" <header>`
* ブラウザーがアクセスできるリクエストヘッダーを示すヘッダーパラメーターのリストです。

#### [!UICONTROL 最大年齢]

* `"maxage" <seconds>`
* A `seconds` プリフライトリクエストの結果をキャッシュできる期間を示すパラメーター。

#### [!UICONTROL サポートされるヘッダー]

* `"supportedheaders" <header>`
* リスト `header` 実際のリクエストをおこなう際に使用できる HTTP ヘッダーを示すパラメーター。

#### [!UICONTROL 許可されるメソッド]

* `"supportedmethods"`
* 実際のリクエストをおこなう際に使用できる HTTP メソッドを示すメソッドパラメーターのリストです。

#### [!UICONTROL 認証情報をサポート]

* `"supportscredentials" <boolean>`
* A `boolean` リクエストに対する応答がブラウザーに公開できるかどうかを示します。 プリフライトリクエストへの応答として使用する場合、これは、資格情報を使用して実際のリクエストを実行できるかどうかを示します。

### 設定例

サイト 1 は、コンテンツが [!DNL GET] リクエスト：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

サイト 2 はより複雑で、承認済みの安全でない要求が必要です。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Dispatcher のキャッシュに関する懸念と設定 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1 以降の応答ヘッダーはキャッシュ可能です。 これにより、キャッシュが可能になります [!DNL CORS] w に沿ったヘッダー [!DNL CORS]リクエストが匿名の場合は、リクエストされたリソースを返します。

一般に、Dispatcher でのコンテンツのキャッシュに関する考慮事項と同じ点を、Dispatcher での CORS 応答ヘッダーのキャッシュに適用できます。 次の表は、 [!DNL CORS] ヘッダー ( [!DNL CORS] リクエスト ) はキャッシュできます。

| Cacheable | 環境 | 認証ステータス | 説明 |
|-----------|-------------|-----------------------|-------------|
| いいえ | AEM パブリッシュ | 認証済み | AEM オーサー上の Dispatcher キャッシュは、静的で、作成されていないアセットに制限されます。 このため、HTTP 応答ヘッダーを含む AEM オーサー上のほとんどのリソースをキャッシュするのは困難で実用的ではありません。 |
| いいえ | AEM パブリッシュ | 認証済み | 認証済みリクエストでは CORS ヘッダーをキャッシュしないでください。 これは、要求元ユーザーの認証/承認ステータスが配信されたリソースに与える影響を判断するのは困難なので、認証済み要求のキャッシュに関しない一般的なガイダンスに従います。 |
| はい | AEM パブリッシュ | 匿名 | Dispatcher でキャッシュ可能な匿名リクエストの応答ヘッダーもキャッシュできるので、今後の CORS リクエストでキャッシュされたコンテンツにアクセスできるようになります。 AEM パブリッシュ時に CORS 設定が変更された場合 **必須** その後、影響を受けるキャッシュされたリソースが無効化されます。 どのキャッシュコンテンツに影響が及ぶかを判断するのは困難なので、コードまたは設定のデプロイメントでは、Dispatcher キャッシュがパージされることが推奨されます。 |

CORS ヘッダーのキャッシュを許可するには、次の設定を、サポートするすべての AEM パブリッシュ dispatcher.any ファイルに追加します。

```
/cache { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

忘れないでください **web サーバーアプリケーションの再起動** 変更後 `dispatcher.any` ファイル。

キャッシュを完全にクリアする必要が生じる可能性があります。これは、 `/cache/headers` 設定を更新しました。

## CORS のトラブルシューティング

ログは以下で使用できます。 `com.adobe.granite.cors`:

* 有効 `DEBUG` 理由の詳細を見る [!DNL CORS] リクエストが拒否されました
* 有効 `TRACE` CORS ハンドラーを介して実行されるすべてのリクエストの詳細を確認するには

### ヒント：

* curl を使用して XHR リクエストを手動で再作成しますが、それぞれが違いを生じる可能性があるので、すべてのヘッダーと詳細を必ずコピーします。一部のブラウザーコンソールでは、curl コマンドをコピーできます。
* 要求が、認証、CSRF トークンフィルター、Dispatcher フィルター、その他のセキュリティレイヤーではなく、CORS ハンドラーによって拒否されたかどうかを確認します。
   * CORS ハンドラーが 200 で応答する場合、 `Access-Control-Allow-Origin` 応答にヘッダーがありません。次の場所にある拒否のログを確認してください： [!DNL DEBUG] in `com.adobe.granite.cors`
* Dispatcher が [!DNL CORS] リクエストが有効になっています
   * 次を確認します。 `/cache/headers` 設定が適用される対象： `dispatcher.any` Web サーバーが正常に再起動されました
   * OSGi または dispatcher.any の設定が変更された後に、キャッシュが正しくクリアされたことを確認します。
* 必要に応じて、リクエストに認証資格情報が存在することを確認します。

## サポート資料

* [クロスオリジンリソース共有ポリシー用のAEM OSGi 設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [クロスオリジンリソース共有 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP アクセス制御 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
