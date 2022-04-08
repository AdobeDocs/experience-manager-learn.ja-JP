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
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# サイトマップ

AEM Sitesのサイトマップを作成して、SEO を強化する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## リソース

+ [AEM Sitemap ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap ドキュメント](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org サイトマップのドキュメント](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org サイトマップインデックスファイルのドキュメント](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 設定

### サイトマップスケジューラーの OSGi 設定

を定義します。 [OSGi ファクトリ設定](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻度 ( [cron 式](http://www.cronmaker.com)) サイトマップは再生成され、AEMにキャッシュされます。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher のフィルタールールを許可

サイトマップのインデックスとサイトマップファイルに対する HTTP 要求を許可します。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Web サーバーの書き換えルール

確認 `.xml` sitemap HTTP 要求は、基になる正しいAEMページにルーティングされます。 URL 短縮化を使用しない場合、または Sling マッピングを使用して URL 短縮化を実現する場合、この設定は不要です。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
