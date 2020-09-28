---
title: AEMでの接触チャネル間のリソース共有(CORS)について理解する
description: Adobe Experience Managerのクロス接触チャネルリソース共有(CORS)は、AEM以外のWebプロパティを容易にし、コンテンツの取得やAEMとの直接のやり取りを行うために、認証済みと非認証の両方のAEMに対してクライアント側の呼び出しを行います。
version: 6.3, 6,4, 6.5
sub-product: 基盤，コンテンツサービス，サイト
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# 接触チャネル間のリソース共有について([!DNL CORS])

Adobe Experience Managerの接触チャネル間のリソース共有([!DNL CORS])は、AEM以外のWebプロパティを容易にし、コンテンツの取得やAEMとの直接のやり取りを行うために、認証済みと非認証の両方で、AEMに対するクライアント側の呼び出しを行います。

## AdobeGraniteクロス接触チャネルリソース共有ポリシーOSGi構成

CORS設定はAEMではOSGi設定ファクトリとして管理され、各ポリシーはファクトリの1つのインスタンスとして表されます。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGraniteクロス接触チャネルリソース共有ポリシーOSGi構成](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### ポリシーの選択

ポリシーは、

* `Allowed Origin` ( `Origin` リクエストヘッダー付き)
* とリクエ `Allowed Paths` ストパスに置き換えます。

これらの値に一致する最初のポリシーが使用されます。 何も見つからない場合、 [!DNL CORS] 要求は拒否されます。

ポリシーがまったく設定されていない場合、 [!DNL CORS] 要求も応答しません。ハンドラーが無効になるため、サーバーの他のモジュールが応答しない限り、事実上拒否され [!DNL CORS]ます。

### Policy プロパティ

#### [!UICONTROL 許可されている接触チャネル]

* `"alloworigin" <origin> | *`
* リソースにアクセスする可能性のあるURIを指定する `origin` パラメーターのリスト。 秘密鍵証明書を持たない要求の場合、サーバーでは*をワイルドカードとして指定できるので、すべての接触チャネルがリソースにアクセスできます。 *CORSを使用しないすべての外国人（攻撃者など）のWebサイト`Allow-Origin: *`がCORSを使用しないリクエストを行うことは、ブラウザーでは厳密に禁止されているので、実稼働環境での使用は絶対にお勧めしません。*

#### [!UICONTROL 許可されている接触チャネル(Regexp)]

* `"alloworiginregexp" <regexp>`
* リソースにアクセスする可能性のあるURIを指定する `regexp` 正規式のリスト。 *正規式は、慎重に構築しないと意図しない一致を引き起こす可能性があり、攻撃者はポリシーに一致するカスタムドメイン名を使用できます。* 通常、他のポリシープロパティを繰り返し設定する場合で `alloworigin`も、を使用して、特定の接触チャネルのホスト名ごとに個別のポリシーを設定することをお勧めします。 接触チャネルが異なると、ライフサイクルや要件が異なるので、明確な分離の利点があります。

#### [!UICONTROL 許可されているパス]

* `"allowedpaths" <regexp>`
* ポリシーが適用されるリソースパスを指定する `regexp` 正規式のリスト。

#### [!UICONTROL 公開されたヘッダ]

* `"exposedheaders" <header>`
* ブラウザーがアクセスを許可するリクエストヘッダーを示すヘッダーパラメーターのリスト。

#### [!UICONTROL 最大年齢]

* `"maxage" <seconds>`
* フライト前の要求の結果をキャッシュできる時間を示す `seconds` パラメーターです。

#### [!UICONTROL サポートされるヘッダー]

* `"supportedheaders" <header>`
* 実際のリクエストを行う際に使用できるHTTPヘッダーを示す `header` パラメーターのリスト。

#### [!UICONTROL 許可されているメソッド]

* `"supportedmethods"`
* 実際の要求を行う際に使用できるHTTPメソッドを示すメソッドパラメーターのリスト。

#### [!UICONTROL 秘密鍵証明書のサポート]

* `"supportscredentials" <boolean>`
* リクエストに対する応答がブラウザーに表示できるかどうかを `boolean` 示します。 プリフライトリクエストへの応答の一部として使用される場合は、資格情報を使用して実際のリクエストを作成できるかどうかを示します。

### 設定例

サイト1は、次の基本的な、匿名でアクセス可能な読み取り専用シナリオで、 [!DNL GET] 要求に応じてコンテンツが使用されます。

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

サイト2はより複雑で、承認済みで安全でない要求が必要です。

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

## ディスパッチャーのキャッシュに関する懸念と設定 {#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1以降の応答ヘッダーは、キャッシュできます。 これにより、リクエストが匿名である限り、リクエストされ [!DNL CORS][!DNL CORS]たリソースに沿ってヘッダーをキャッシュできます。

一般に、ディスパッチャーのコンテンツをキャッシュする場合と同じ考慮事項を、ディスパッチャーでCORS応答ヘッダーをキャッシュする場合に適用できます。 次の表に、 [!DNL CORS][!DNL CORS] ヘッダー（およびその結果要求）をキャッシュできるタイミングを示します。

| キャッシュ可能 | 環境 | 認証状態 | 説明 |
|-----------|-------------|-----------------------|-------------|
| 不可 | AEM Publish | 認証済み | AEM Authorのディスパッチャーキャッシュは、静的で作成されていないアセットに制限されます。 これにより、HTTP応答ヘッダーを含むほとんどのリソースをAEM Authorにキャッシュするのが困難で実用的ではありません。 |
| 不可 | AEM Publish | 認証済み | 認証済みの要求ではCORSヘッダーのキャッシュを避けます。 これは、要求元ユーザーの認証/認証状態が配信されたリソースにどのように影響するかを判断するのが困難なため、認証済み要求をキャッシュしない場合の一般的なガイダンスに沿っています。 |
| 可 | AEM Publish | 匿名 | ディスパッチャーでキャッシュ可能な匿名要求は、応答ヘッダーもキャッシュできます。これにより、今後のCORS要求でキャッシュされたコンテンツにアクセスできるようになります。 AEMの発行に対するCORS設定の変更 **は** 、影響を受けるキャッシュリソースの無効化の後に行う必要があります。 コードまたは設定のデプロイメントでは、どのキャッシュコンテンツに影響が及ぶかを判断するのは困難なため、ディスパッチャーキャッシュは削除されます。 |

CORSヘッダーのキャッシュを許可するには、サポートするすべてのAEM発行ディスパッチャー.anyファイルに次の設定を追加します。

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

フ **ァイルに変更を加えた後は、Webサーバーアプリケーションを**`dispatcher.any` 再起動する必要があります。

設定の更新後、次の要求でヘッダーが適切にキャッシュされるように、キャッシュを完全にクリアする必要がある場合があり `/headers` ます。

## CORSのトラブルシューティング

ログは次の場所で利用でき `com.adobe.granite.cors`ます。

* 要求 `DEBUG`[!DNL CORS] が拒否された理由の詳細を確認する
* CORSハ `TRACE` ンドラーを経由するすべての要求の詳細を表示する

### ヒント：

* curlを使用してXHRリクエストを手動で再作成しますが、各リクエストで変更が生じる可能性があるので、必ずすべてのヘッダーと詳細をコピーしてください。一部のブラウザコンソールでは、curlコマンドをコピーできます
* 要求が、認証、CSRFトークンフィルター、ディスパッチャーフィルター、その他のセキュリティ層ではなく、CORSハンドラーによって拒否されたかどうかを確認します
   * CORSハンドラーが200を使用して応答し、応答に `Access-Control-Allow-Origin`[!DNL DEBUG] ヘッダーがない場合は、ログの「 `com.adobe.granite.cors`
* リクエストのディスパッチャーキャッシュが有効になっ [!DNL CORS] ている場合
   * 設定がに適用され、Webサーバーが正常に再起動され `/headers``dispatcher.any` たことを確認します
   * OSGiまたはディスパッチャーの設定が変更された後に、キャッシュが正しくクリアされたことを確認します。
* 必要に応じて、認証資格情報がリクエストに存在することを確認します。

## サポート資料

* [クロス接触チャネルリソース共有ポリシーのAEM OSGi設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [接触チャネル間のリソース共有(W3C)](https://www.w3.org/TR/cors/)
* [HTTPアクセス制御(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
