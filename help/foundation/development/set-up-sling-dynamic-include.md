---
title: AEM 用の Sling Dynamic Include の設定
description: Apache HTTP web サーバー上で動作する AEM Dispatcher と Apache Sling Dynamic Include をインストールし、使用するためのビデオウォークスルーです。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '230'
ht-degree: 100%

---

# [!DNL Sling Dynamic Include] の設定

[!DNL Apache HTTP Web Server] 上で動作する [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja) と [!DNL Apache Sling Dynamic Include] をインストールし、使用するための手順を紹介するビデオです。

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 最新バージョンの AEM Dispatcher がローカルにインストールされていることを確認します。

1. [[!DNL Sling Dynamic Include] バンドル](https://sling.apache.org/downloads.cgi)をダウンロードしてインストールします。
1. **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration** で [!DNL OSGi Configuration Factory] を介して [!DNL Sling Dynamic Include] を設定します。

   または、AEM コードベースに追加するには、次の場所に適切な **sling:OsgiConfig** ノードを作成します。

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

1. （オプション）最後の手順を繰り返して、[編集可能なテンプレートのロックされた（初期）コンテンツ ](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html) のコンポーネントを、[!DNL SDI] 経由でも提供できるようにします。追加設定の理由は、編集可能なテンプレートのロックされたコンテンツが、`/content` ではなく `/conf` から提供されるためです。

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

1. [!DNL Apache HTTPD Web server] の `httpd.conf` ファイルを更新して [!DNL Include] モジュールを有効にします。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. include ディレクティブを尊重するように [!DNL vhost] ファイルを更新します。

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

1. （1）`nocache` セレクターをサポートし、（2）TTL サポートを有効にするように、dispatcher.any 構成ファイルを更新します。

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
   > 上記の glob `*.nocache.html*` ルールで末尾の `*` を省略すると、[サブリソースのリクエストで問題](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)が発生する可能性があります。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 構成ファイルまたは `dispatcher.any` を変更した後は、必ず [!DNL Apache HTTP Web Server] を再起動してください。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>エッジサイドインクルード（ESI）を提供するために [!DNL Sling Dynamic Includes] を使用している場合は、関連する[レスポンスヘッダーをディスパッチャーキャッシュ ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#CachingHTTPResponseHeaders)にキャッシュしてください。ヘッダーの例を次に示します。
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;有効期限&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>

## サポート資料

* [Sling Dynamic Include バンドルをダウンロード](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include ドキュメント](https://github.com/Cognifide/Sling-Dynamic-Include)
