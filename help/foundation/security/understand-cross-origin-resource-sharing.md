---
title: AEMを使用したクロスオリジンリソース共有(CORS)について
description: Adobe Experience Managerのクロスオリジンリソース共有(CORS)は、AEM以外のWebプロパティを容易にして、AEMに対するクライアント側の呼び出し（認証済みと未認証の両方）をおこない、コンテンツを取得したり、AEMと直接やり取りします。
version: 6.3, 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: セキュリティ
role: Developer
level: Intermediate
source-git-commit: 1c99c319fba5048904177fc82c43554b0cf0fc15
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 1%

---


# クロスオリジンリソース共有([!DNL CORS])を理解する

Adobe Experience Managerのクロスオリジンリソース共有([!DNL CORS])は、AEM以外のWebプロパティを容易にして、AEMに対するクライアント側の呼び出し（認証済みと未認証の両方）を実行し、コンテンツを取得したり、AEMと直接やり取りします。

## AdobeGraniteクロスオリジンリソース共有ポリシーのOSGi設定

CORS設定は、AEMでOSGi設定ファクトリとして管理され、各ポリシーはファクトリの1つのインスタンスとして表されます。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGraniteクロスオリジンリソース共有ポリシーのOSGi設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### ポリシーの選択

ポリシーは、

* `Allowed Origin` リクエストヘッ `Origin` ダーを使用
* と`Allowed Paths`をリクエストパスに置き換えます。

これらの値に一致する最初のポリシーが使用されます。 何も見つからない場合、[!DNL CORS]リクエストは拒否されます。

ポリシーがまったく設定されていない場合、[!DNL CORS]要求にも応答しません。これは、[!DNL CORS]に応答するサーバーの他のモジュールがない限り、ハンドラーが無効になり、事実上拒否されるからです。

### Policy プロパティ

#### [!UICONTROL 許可されたオリジン]

* `"alloworigin" <origin> | *`
* リソースにアクセスする可能性のあるURIを指定する`origin`パラメーターのリスト。 資格情報のないリクエストの場合、サーバーはワイルドカードとして*を指定できるので、任意の接触チャネルがリソースにアクセスできます。 *CORSを使用しない要求はブラウザーによって厳密に禁止されるの `Allow-Origin: *` で、実稼動環境での使用は絶対にお勧めしません。*

#### [!UICONTROL 許可されたオリジン（正規表現）]

* `"alloworiginregexp" <regexp>`
* リソースにアクセスできるURIを指定する`regexp`正規表現のリスト。 *正規表現は、慎重に構築しなければ意図しない一致を引き起こし、攻撃者がポリシーと一致するカスタムドメイン名を使用できる可能性があります。* 通常は、他のポリシープロパティの設定を繰り返しおこなう場合でも、を使用し `alloworigin`て、特定のオリジンホスト名ごとに個別のポリシーを設定することをお勧めします。起源が異なると、ライフサイクルや要件が異なり、明確な分離の恩恵を受けます。

#### [!UICONTROL 許可されているパス]

* `"allowedpaths" <regexp>`
* ポリシーが適用されるリソースパスを指定する`regexp`正規表現のリスト。

#### [!UICONTROL 公開されたヘッダー]

* `"exposedheaders" <header>`
* ブラウザーがアクセスできるリクエストヘッダーを示すヘッダーパラメーターのリストです。

#### [!UICONTROL 最高年齢]

* `"maxage" <seconds>`
* プリフライトリクエストの結果をキャッシュできる時間を示す`seconds`パラメーター。

#### [!UICONTROL サポートされるヘッダー]

* `"supportedheaders" <header>`
* 実際のリクエストをおこなう際に使用できるHTTPヘッダーを示す`header`パラメーターのリスト。

#### [!UICONTROL 許可されるメソッド]

* `"supportedmethods"`
* 実際のリクエストをおこなう際に使用できるHTTPメソッドを示すメソッドパラメーターのリストです。

#### [!UICONTROL 資格情報をサポート]

* `"supportscredentials" <boolean>`
* リクエストへの応答がブラウザーに公開できるかどうかを示す`boolean`。 プリフライトリクエストへの応答の一部として使用される場合、これは、実際のリクエストが資格情報を使用して実行できるかどうかを示します。

### 設定例

サイト1は、[!DNL GET]リクエストを通じてコンテンツを利用する、匿名でアクセス可能な読み取り専用の基本的なシナリオです。

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

サイト2はより複雑で、承認済みの安全でない要求が必要です。

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

## Dispatcherのキャッシュに関する問題と設定{#dispatcher-caching-concerns-and-configuration}

Dispatcher 4.1.1以降の応答ヘッダーはキャッシュ可能です。 これにより、リクエストが匿名である限り、[!DNL CORS]リクエストされたリソースに沿って[!DNL CORS]ヘッダーをキャッシュできます。

一般に、Dispatcherでのコンテンツのキャッシュに関する同じ考慮事項は、DispatcherでのCORS応答ヘッダーのキャッシュに適用できます。 次の表は、 [!DNL CORS]ヘッダー（したがって[!DNL CORS]リクエスト）がキャッシュ可能なタイミングを示しています。

| キャッシュ可能 | 環境 | 認証ステータス | 説明 |
|-----------|-------------|-----------------------|-------------|
| 不可 | AEM パブリッシュ | 認証済み | AEMオーサー上のDispatcherのキャッシュは、静的で、作成されていないアセットに制限されます。 これにより、HTTP応答ヘッダーを含むAEMオーサー上のほとんどのリソースをキャッシュするのが困難で実用的ではありません。 |
| 不可 | AEM パブリッシュ | 認証済み | 認証済みリクエストではCORSヘッダーをキャッシュしないでください。 これは、要求元ユーザーの認証/承認ステータスが配信されたリソースに与える影響を判断するのは困難なので、認証済み要求のキャッシュに関しない一般的なガイダンスに従います。 |
| 可 | AEM パブリッシュ | 匿名 | Dispatcherでキャッシュ可能な匿名リクエストの応答ヘッダーもキャッシュでき、今後のCORSリクエストでキャッシュされたコンテンツにアクセスできるようになります。 AEMパブリッシュでのCORS設定の変更には、影響を受けるキャッシュリソースを無効にする必要があります。**** キャッシュされたコンテンツを特定するのは困難なので、コードまたは設定のデプロイメントではDispatcherキャッシュがパージされるのがベストプラクティスです。 |

CORSヘッダーのキャッシュを許可するには、サポートするすべてのAEMパブリッシュdispatcher.anyファイルに次の設定を追加します。

```
/cache { 
  ...
  /headers {
      "Origin",
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

`dispatcher.any`ファイルに変更を加えた後、必ず&#x200B;**Webサーバーアプリケーション**&#x200B;を再起動してください。

`/cache/headers`設定の更新後に、次の要求でヘッダーが適切にキャッシュされるように、キャッシュを完全にクリアする必要が生じる可能性があります。

## CORSのトラブルシューティング

ログは`com.adobe.granite.cors`の下にあります。

* `DEBUG`を有効にして、[!DNL CORS]要求が拒否された理由の詳細を確認します。
* `TRACE`を有効にして、CORSハンドラーを介するすべてのリクエストの詳細を確認します。

### ヒント：

* curlを使用してXHRリクエストを手動で再作成しますが、それぞれが違いを生じる可能性があるので、必ずすべてのヘッダーと詳細をコピーしてください。一部のブラウザーコンソールでは、curlコマンドのコピーが可能です。
* 要求が、認証、CSRFトークンフィルター、Dispatcherフィルター、その他のセキュリティレイヤーではなく、CORSハンドラーによって拒否されたかどうかを確認します
   * CORSハンドラーが200で応答し、応答に`Access-Control-Allow-Origin`ヘッダーがない場合は、 `com.adobe.granite.cors`の[!DNL DEBUG]で拒否のログを確認します。
* [!DNL CORS]要求のDispatcherキャッシュが有効になっている場合
   * `/cache/headers`設定が`dispatcher.any`に適用され、Webサーバーが正常に再起動されたことを確認します。
   * OSGiまたはdispatcher.anyの設定が変更された後にキャッシュが正しくクリアされたことを確認します。
* 必要に応じて、リクエストに認証資格情報が存在することを確認します。

## サポート資料

* [クロスオリジンリソース共有ポリシー用のAEM OSGi設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [クロスオリジンリソース共有(W3C)](https://www.w3.org/TR/cors/)
* [HTTPアクセス制御(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
