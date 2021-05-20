---
title: AEMを使用したクロスオリジンリソース共有(CORS)の開発
description: CORSを活用して、クライアント側JavaScriptを使用して外部WebアプリケーションからAEMコンテンツにアクセスする短い例です。
version: 6.3, 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
topic: セキュリティ
role: Developer
level: Beginner
feature: null
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---


# クロスオリジンリソース共有(CORS)の開発

[!DNL CORS]を活用して、クライアント側JavaScriptを使用して外部WebアプリケーションからAEMコンテンツにアクセスする短い例です。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

このビデオでは、

* **www.example.commapsを** 介してlocalhostに  `/etc/hosts`
* **aem-publish.localmapsを** ローカルホスト（経由）に  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (のSimpleHTTPServer [[!DNL Python]のラッパー)は、HTMLページをポ](https://docs.python.org/2/library/simplehttpserver.html)ート8000を介して提供します。
* [!DNL AEM Dispatcher] は、2.4で実行さ [!DNL Apache HTTP Web Server] れ、に対するリバースプロキシ要求を実 `aem-publish.local` 行しま `localhost:4503`す。

詳しくは、AEM](./understand-cross-origin-resource-sharing.md)の[Understanding Cross-Origin Resource Sharing(CORS)を参照してください。

## www.example.com HTMLおよびJavaScript

このWebページには

1. ボタンをクリックしたとき
1. [!DNL AJAX GET]リクエストを`http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`に送信
1. JSON応答から`jcr:title`を取得します
1. `jcr:title`をDOMに挿入します。

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGiファクトリ設定

[!DNL Cross-Origin Resource Sharing]のOSGi設定ファクトリは、次の場所から入手できます。

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher設定{#dispatcher-configuration}

キャッシュされたコンテンツでCORSヘッダーのキャッシュと提供を可能にするには、[/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)の後に、AEMパブリッシュの`dispatcher.any`ファイルをサポートするすべてのに追加します。

```
/cache { 
  ...
  /clientheaders {
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

**ファイルに変更を加えた後、Webサ** ーバーアプリケーションを再起動 `dispatcher.any` します。

`/clientheaders`設定の更新後に、次の要求でヘッダーが適切にキャッシュされるようにするには、キャッシュを完全にクリアする必要がある可能性があります。

## サポート資料{#supporting-materials}

* [クロスオリジンリソース共有ポリシー用のAEM OSGi設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [macOS用のSimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) （Windows/macOS/Linux互換）

* [AEMでのクロスオリジンリソース共有(CORS)について](./understand-cross-origin-resource-sharing.md)
* [クロスオリジンリソース共有(W3C)](https://www.w3.org/TR/cors/)
* [HTTPアクセス制御(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

