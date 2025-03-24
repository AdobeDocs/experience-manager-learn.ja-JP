---
title: AEM を使用したクロスオリジンリソース共有（CORS）の開発
description: CORS を活用して、クライアントサイド JavaScript を使用して外部 web アプリケーションから AEM コンテンツにアクセスする簡単な例です。
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 100%

---

# クロスオリジンリソース共有（CORS）に対応する開発

[!DNL CORS] を活用して、クライアントサイド JavaScript で外部 web アプリケーションから AEM コンテンツにアクセスする簡単な例です。この例では、CORS OSGi 設定を使用して、AEM での CORS アクセスを有効にします。OSGi 設定アプローチは、次の場合に有効です。

* 単一オリジンが AEM パブリッシュコンテンツにアクセスしている
* AEM オーサーには CORS アクセスが必要である

AEM パブリッシュへのマルチオリジンアクセスが必要な場合は、[このドキュメント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=ja#dispatcher-configuration)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

このビデオの内容は次のとおりです。

* **www.example.com** が `/etc/hosts` を介してローカルホストにマッピングされます。
* **aem-publish.local** が `/etc/hosts` を介してローカルホストにマッピングされます。
* SimpleHTTPServer（[[!DNL Python] の SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) のラッパー）がポート 8000 で HTML ページを提供しています。
   * _Mac App Store では入手できなくなりました。[Jeeves](https://apps.apple.com/jp/app/jeeves-local-http-server/id980824182?mt=12) など、類似のものを使用します。_
* [!DNL AEM Dispatcher] が [!DNL Apache HTTP Web Server] 2.4 で動作しており、`aem-publish.local` へのリクエストを `localhost:4503` にリバースプロキシしています。

詳しくは、[AEM でのクロスオリジンリソース共有（CORS）について](./understand-cross-origin-resource-sharing.md)を参照してください。

## www.example.com の HTML と JavaScript

この web ページには、次のようなロジックがあります。

1. ボタンのクリック時に、
1. `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json` に対して [!DNL AJAX GET] リクエストを実行します。
1. JSON 応答から `jcr:title` フォームを取得します。
1. DOM に `jcr:title` を挿入します。

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

## OSGi ファクトリ設定

[!DNL Cross-Origin Resource Sharing] の OSGi 設定ファクトリは、次の方法で入手できます。

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

## Dispatcher 設定 {#dispatcher-configuration}

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

キャッシュされたコンテンツの CORS ヘッダーをキャッシュし提供できるようにするには、AEM パブリッシュの `dispatcher.any` ファイルに次の [/cache /headers 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#caching-http-response-headers)を追加します。

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

`dispatcher.any` ファイルを変更した後、**web サーバーアプリケーションを再起動**&#x200B;します。

`/cache /headers` 設定更新後の次のリクエストでヘッダーが適切にキャッシュされるようにするには、キャッシュを完全にクリアすることが必要です。

## サポート資料 {#supporting-materials}

* [macOS 版 Jeeves](https://apps.apple.com/jp/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html)（Windows／macOS／Linux 対応）

* [AEM でのクロスオリジンリソース共有（CORS）について](./understand-cross-origin-resource-sharing.md)
* [クロスオリジンリソース共有（W3C）](https://www.w3.org/TR/cors/)
* [HTTP アクセス制御（Mozilla MDN）](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Access_control_CORS)
