---
title: サイトマップ
description: AEM Sitesのサイトマップを作成して、SEO を強化する方法を説明します。
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# サイトマップ

AEM Sitesのサイトマップを作成して、SEO を強化する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## リソース

+ [AEM Sitemap ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling サイトマップドキュメント](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM Core WCM Components Github](https://github.com/adobe/aem-core-wcm-components)
   + v2.17.6で追加されたサイトマップ機能
+ [Sitemap.org サイトマップのドキュメント](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org サイトマップインデックスファイルのドキュメント](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 設定

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

を定義します。 [OSGi ファクトリ設定](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻度 ( [cron 式](http://www.cronmaker.com)) サイトマップは、再生成され、AEMにキャッシュされます。

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

サイトマップのインデックスとサイトマップファイルに対する HTTP 要求を許可します。

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

確認 `.xml` サイトマップ HTTP 要求は、基になる正しいAEMページにルーティングされます。 URL 短縮化を使用しない場合や、Sling マッピングを使用して URL 短縮化を実行する場合は、この設定は必要ありません。

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
