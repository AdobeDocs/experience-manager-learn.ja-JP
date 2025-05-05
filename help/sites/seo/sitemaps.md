---
title: サイトマップ
description: AEM Sites のサイトマップを作成して SEO を強化する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '234'
ht-degree: 100%

---

# サイトマップ

AEM Sites のサイトマップを作成して SEO を強化する方法について説明します。

>[!WARNING]
>
>このビデオでは、サイトマップ内の相対 URI の使用について説明します。 サイトマップは、[絶対 URL を使用する必要があります](https://sitemaps.org/protocol.html)。 絶対 URL を有効にする方法については、以下のビデオでは説明していないため、[設定](#absolute-sitemap-urls)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3454362?quality=12&learn=on&captions=jpn)

## 設定

### サイトマップの絶対 URL{#absolute-sitemap-urls}

AEM のサイトマップは、[Sling マッピング](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)を使用して絶対 URL をサポートしています。 これは、サイトマップを生成する AEM サービス（通常は AEM パブリッシュサービス）にマッピングノードを作成することで行われます。

次のように、例えば `https://wknd.com` の Sling マッピングノードは、`/etc/map/https` で定義できます。

| パス | プロパティ名 | プロパティタイプ | プロパティ値 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 文字列 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 文字列 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 文字列 | `wknd.com/$1` |

以下のスクリーンショットは、同様の設定を示していますが、`http://wknd.local`（`http` で実行中のローカルホスト名のマッピング）を対象としています。

![サイトマップの絶対 URL の設定](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### サイトマップスケジューラーの OSGi 設定

（[cron 式](https://cron.help/)を使用して）サイトマップが生成／再生成され、AEM にキャッシュされる頻度について、[OSGi ファクトリ設定](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler)を定義します。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher のフィルター許可ルール

サイトマップのインデックスとサイトマップファイルに対する HTTP リクエストを許可します。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache web サーバーの書き換えルール

`.xml` サイトマップの HTTP リクエストが、基になる正しい AEM ページにルーティングされるようにします。 URL 短縮化を使用しない場合、または Sling マッピングを使用して URL 短縮化を実行する場合、この設定は不要です。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## リソース

+ [AEM サイトマップのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=ja)
+ [Apache Sling サイトマップのドキュメント](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org サイトマップのドキュメント](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org サイトマップインデックスファイルのドキュメント](https://www.sitemaps.org/protocol.html#index)
+ [Cron ヘルパー](https://cron.help/)