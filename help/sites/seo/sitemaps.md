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
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 6%

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

を定義します。 [OSGi ファクトリ設定](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻度 ( [cron 式](http://www.cronmaker.com)) サイトマップは、AEMで再生成およびキャッシュされます。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### 絶対サイトマップ URL

AEM sitemap は、 [Sling マッピング](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). これは、サイトマップを生成するAEMサービス（通常は AEM パブリッシュサービス）にマッピングノードを作成することでおこなわれます。

の Sling マッピングノード定義の例 `https://wknd.com` は以下で定義できます。 `/etc/map/https` 次のように指定します。

| パス  | プロパティ名 | プロパティタイプ | プロパティ値 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 文字列 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 文字列 | `wknd.com/$1` |

以下のスクリーンショットは、 `http://wknd.local` （で実行中のローカルホスト名マッピング） `http`) をクリックします。

![サイトマップの絶対 URL 設定](../assets/sitemaps/sitemaps-absolute-urls.jpg)


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
