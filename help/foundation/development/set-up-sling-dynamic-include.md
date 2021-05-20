---
title: AEM用のSling Dynamic Includeの設定
description: Apache HTTP Webサーバー上で動作するAEM Dispatcherと共にApache Sling Dynamic Includeをインストールおよび使用する方法に関するビデオのウォークスルーです。
version: 6.3, 6.4, 6.5
sub-product: 基盤、サイト
feature: API
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 9%

---


# 設定 [!DNL Sling Dynamic Include]

[!DNL Apache HTTP Web Server]上で動作する[!DNL Apache Sling Dynamic Include]と[AEM Dispatcher](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/dispatcher.html)をインストールして使用するビデオのウォークスルーです。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 最新バージョンのAEM Dispatcherがローカルにインストールされていることを確認します。

1. [[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi)をダウンロードしてインストールします。
1. **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;の[!DNL OSGi Configuration Factory]を使用して[!DNL Sling Dynamic Include]を設定します。

   または、AEMコードベースにを追加するには、次の場所に適切な&#x200B;**sling:OsgiConfig**&#x200B;ノードを作成します。

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

1. （オプション）最後の手順を繰り返して、編集可能なテンプレート](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html)の[ロックされた（初期）コンテンツ上のコンポーネントも[!DNL SDI]経由で提供できるようにします。 追加の設定の理由は、編集可能なテンプレートのロックされたコンテンツが`/content`ではなく`/conf`から提供されるからです。

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

1. [!DNL Apache HTTPD Web server]の`httpd.conf`ファイルを更新して、[!DNL Include]モジュールを有効にします。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. インクルードディレクティブに従うように[!DNL vhost]ファイルを更新します。

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

1. dispatcher.any設定ファイルを更新して、(1) `nocache`セレクターをサポートし、(2)TTLサポートを有効にします。

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
   > 上記のglobの`*.nocache.html*`ルールで末尾の`*`をオフのままにすると、サブリソース](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)のリクエストで[の問題が発生する可能性があります。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 設定ファイルまたは`dispatcher.any`に変更を加えた後は、必ず[!DNL Apache HTTP Web Server]を再起動してください。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>エッジ側インクルード(ESI)を提供するために[!DNL Sling Dynamic Includes]を使用する場合は、関連する[応答ヘッダーをDispatcherキャッシュ](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)にキャッシュします。 ヘッダーの例を次に示します。
>
>* 「キャッシュ制御」
>* 「Content-Disposition」
>* &quot;Content-Type&quot;
>* &quot;有効期限&quot;
>* &quot;最終変更日&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;最終変更日&quot;

>



## サポート資料

* [Sling Dynamic Includeバンドルのダウンロード](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Includeドキュメント](https://github.com/Cognifide/Sling-Dynamic-Include)
