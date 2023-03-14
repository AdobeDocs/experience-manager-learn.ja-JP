---
title: AEMを使用したクロスオリジンリソース共有 (CORS) の開発
description: CORS を活用して、クライアント側 JavaScript を使用して外部 Web アプリケーションからAEMコンテンツにアクセスする短い例です。
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 1c8f8d8aeee60c06c9d0d496fd86717ab8fe8295
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# クロスオリジンリソース共有 (CORS) 用の開発

活用の短い例 [!DNL CORS] クライアント側 JavaScript を使用して、外部 Web アプリケーションからAEMコンテンツにアクセスする。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

このビデオでは、

* **www.example.com** はを介して localhost にマッピングされます。 `/etc/hosts`
* **aem-publish.local** はを介して localhost にマッピングされます。 `/etc/hosts`
* SimpleHTTPServer ( [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) は、ポート 8000 を介してHTMLページを提供します。
   * _Mac App Storeでは使用できなくなりました。 次のような類似のを使用します。 [ジーブス](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 実行中 [!DNL Apache HTTP Web Server] 2.4.に対するリバースプロキシ要求 `aem-publish.local` から `localhost:4503`.

詳しくは、 [AEMでのクロスオリジンリソース共有 (CORS) について](./understand-cross-origin-resource-sharing.md).

## www.example.comHTMLと JavaScript

この Web ページには

1. 「 」ボタンをクリックしたとき
1. を [!DNL AJAX GET] ～を要求する `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. を取得します。 `jcr:title` JSON 応答を形成する
1. を挿入します。 `jcr:title` DOM に

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

の OSGi 設定ファクトリ [!DNL Cross-Origin Resource Sharing] は、次の場所から使用できます。

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

キャッシュされたコンテンツでの CORS ヘッダーのキャッシュと提供を許可するには、以下を追加します。 [/clientheaders 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) を、AEM パブリッシュをサポートするすべての `dispatcher.any` ファイル。

```
/myfarm { 
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

**Web サーバーアプリケーションを再起動します** 変更後 `dispatcher.any` ファイル。

キャッシュを完全にクリアする必要が生じる可能性があります。これは、 `/clientheaders` 設定を更新しました。

## サポート資料 {#supporting-materials}

* [クロスオリジンリソース共有ポリシー用のAEM OSGi 設定ファクトリ](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [macOSのジーブ](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (Windows/macOS/Linux 互換 )

* [AEMでのクロスオリジンリソース共有 (CORS) について](./understand-cross-origin-resource-sharing.md)
* [クロスオリジンリソース共有 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP アクセス制御 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
