---
title: AEM用の Sling Dynamic Include の設定
description: Apache HTTP Web サーバー上で動作するAEM Dispatcher と共に Apache Sling Dynamic Include をインストールして使用する方法に関するビデオのウォークスルーです。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 8%

---

# 設定 [!DNL Sling Dynamic Include]

のインストールと使用に関するビデオウォークスルー [!DNL Apache Sling Dynamic Include] と [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja) 実行中 [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 最新バージョンのAEM Dispatcher がローカルにインストールされていることを確認します。

1. をダウンロードしてインストールする [[!DNL Sling Dynamic Include] バンドル](https://sling.apache.org/downloads.cgi).
1. 設定 [!DNL Sling Dynamic Include] 経由 [!DNL OSGi Configuration Factory] 時刻 **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   または、AEMコードベースにを追加するには、適切な **sling:OsgiConfig** ノード：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. （オプション）最後の手順を繰り返して、上のコンポーネントを許可します。 [編集可能なテンプレートのロック（初期）されたコンテンツ](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 経由で供される [!DNL SDI] 同様に。 追加の設定の理由は、編集可能テンプレートのロックされたコンテンツが次の場所から提供されるからです。 `/conf` の代わりに `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. 更新 [!DNL Apache HTTPD Web server]&#39;s `httpd.conf` 有効にするファイル [!DNL Include] モジュール。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. を更新します。 [!DNL vhost] include ディレクティブを尊重するファイル。

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. dispatcher.any 設定ファイルを (1) に対応するように更新します。 `nocache` セレクターと (2) は TTL サポートを有効にします。

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > 末尾の `*` 暗がりで `*.nocache.html*` 上のルールは、次のようになります。 [サブリソースのリクエストの問題](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 常に再起動 [!DNL Apache HTTP Web Server] 設定ファイルまたは `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>次を使用する場合： [!DNL Sling Dynamic Includes] エッジサイドインクルード (ESI) を提供する場合は、関連する [dispatcher キャッシュの応答ヘッダー](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). ヘッダーの例を次に示します。
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;有効期限&quot;
>* &quot;最終変更日&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;最終変更日&quot;
>


## サポート資料

* [Sling Dynamic Include バンドルをダウンロード](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include ドキュメント](https://github.com/Cognifide/Sling-Dynamic-Include)
