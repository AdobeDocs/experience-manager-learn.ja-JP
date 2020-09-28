---
title: AEM用のSling動的インクルードの設定
description: Apache HTTP Webサーバー上で実行されるAEMディスパッチャーと共にApache Sling Dynamic Includeをインストールして使用するビデオのウォークスルーです。
version: 6.3, 6.4, 6.5
sub-product: 基盤、サイト
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 9%

---


# 設定 [!DNL Sling Dynamic Include]

AEM Dispatcherを実行してインストールおよび使用する方法 [!DNL Apache Sling Dynamic Include] に関するビデオのウォークスルー [](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/dispatcher.html)[!DNL Apache HTTP Web Server]です。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> AEM Dispatcherの最新バージョンがローカルにインストールされていることを確認します。

1. バンドルをダウンロードしてインスト [[!DNL Sling Dynamic Include] ールします](https://sling.apache.org/downloads.cgi)。
1. http://&lt; [!DNL Sling Dynamic Include] host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configurationを [!DNL OSGi Configuration Factory] 使用して設定を行います ****。

   AEMコードベースに追加するには、次の場所で適切な **sling:OsgiConfig** ノードを作成します。

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

1. （オプション）最後の手順を繰り返して、編集可能なテンプレートの [ロックされた（初期の）コンテンツのコンポーネントも](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 、を介して提供でき [!DNL SDI] るようにします。 追加の設定が行われる理由は、編集可能なテンプレートのロックされたコンテンツが、ではなく、から提供され `/conf` ること `/content`です。

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

1. の [!DNL Apache HTTPD Web server]ファイルを更新して `httpd.conf`[!DNL Include] モジュールを有効にします。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. インクルードディレクティブに従うように [!DNL vhost] ファイルを更新します。

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

1. dispatcher.any設定ファイルを更新して、(1)セレクターをサポートし、(2) TTL `nocache` を有効にします。

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
   > 上記のグローブ `*` ルールの末尾をオフにしたままにすると、サブリソースの要求で `*.nocache.html*` 問題が発生する可能性があります [](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 設定ファイルまたはを変更 [!DNL Apache HTTP Web Server] した後は、必ず再起動してく `dispatcher.any`ださい。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>エッジ側インクルード(ESI) [!DNL Sling Dynamic Includes] を提供するためにを使用する場合は、関連する [応答ヘッダーをディスパッチャーキャッシュにキャッシュします](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)。 使用可能なヘッダーは次のとおりです。
>
>* 「Cache-Control」
>* 「Content-Disposition」
>* &quot;Content-Type&quot;
>* &quot;有効期限&quot;
>* 「最終変更日」
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* 「最終変更日」

>



## サポート資料

* [Sling動的インクルードバンドルのダウンロード](https://sling.apache.org/downloads.cgi)
* [Apache Sling動的インクルードドキュメント](https://github.com/Cognifide/Sling-Dynamic-Include)
