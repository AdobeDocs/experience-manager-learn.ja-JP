---
title: AMS Dispatcher 読み取り専用または不変ファイル
description: 一部のファイルが読み取り専用または編集不可の理由と、必要な機能の変更方法の理解
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---


# AMS の読み取り専用ファイルまたは不変ファイル

[目次](./overview.md)

[&lt; — 前：共通ログ](./common-logs.md)

## 説明

このドキュメントでは、ロックされているファイルと変更されないファイル、および適切な構成設定を行う方法について説明します。

AMS がシステムをプロビジョニングする際に、すべてが機能し、安全になるベースライン設定を展開します。  これらは、AMS が確実に機能とセキュリティの基準として保証したいと考えているものです。  これを実現するために、一部のファイルは読み取り専用としてマークされ、変更を避けるために不変です。

レイアウトを使用しても、動作を変更したり、必要な変更を上書きしたりすることはできません。  これらのファイルを変更する代わりに、元のファイルを置き換える独自のファイルをオーバーレイします。

これにより、AMS が最新の修正とセキュリティの強化で Dispatcher にパッチを適用しても、ファイルが変更されないことを確認できます。  その後も、改善点のメリットを活用し、必要な変更のみを採用できます。
![ボールがレーンを下りていくボーリングレーンを表示します。  ボールには、「 」という単語が付いた矢印が表示されます。  樋のバンパーは上に上がり、その上に不変ファイルという単語があります。](assets/immutable-files/bowling-file-immutability.png "ボウリングファイル不変性")
上の図で示すように、不変ファイルは、ゲームを再生するのを止めていません。  彼らは、あなたのパフォーマンスを傷つけるのを止め、あなたを車線に留める。  この方法では、次の主要な機能をいくつか利用できます。

- カスタマイズは、独自の安全なスペースで処理されます
- カスタム変更のオーバーレイ (AEMのオーバーレイメソッドのもの )
- AMS 設定のパッチ適用は、カスタマイズを変更せずに実行できます
- 基本インストールとカスタマイズされた設定のテストは、同時に行うことができ、問題がカスタマイズやその他の原因で発生しているかどうかを判別できます。どのファイルが原因か


Dispatcher と共にデプロイされるファイルの一般的なリストを次に示します。

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

不変ファイルを判断するには、Dispatcher で次のコマンドを実行して、

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

次に、不変ファイルに対する応答のサンプルを示します。

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## 変更方法

### 変数

変数を使用すると、設定ファイル自体を変更せずに、機能を変更できます。  設定の特定の要素は、変数の値を調整して調整できます。  ファイルからハイライトできる例を 1 つ示します。 `/etc/httpd/conf.d/dispatcher_vhost.conf` は次のように表示されます。

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

DispatcherLogLevel ディレクティブに次の変数がある方法を確認する `DISP_LOG_LEVEL` 通常の値ではなく、通常の値が表示されます。  コードのこのセクションの上には、変数ファイルに対する include ステートメントも表示されます。  変数ファイル `/etc/httpd/conf.d/variables/ams_default.vars` は、次に見たい場所です。  変数ファイルの内容を次に示します。

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

上記のように、 `DISP_LOG_LEVEL` 変数 `info`.  これを調整して、トレースやデバッグを行ったり、数値やレベルを選択したりできます。  現在は、ログレベルを制御するすべての場所が自動的に調整されます。

### Overlay メソッド

トップレベルのインクルードファイルを理解してください。これらのファイルはカスタマイズを開始する際に使用します。  簡単な例から始めるために、この Dispatcher を指す新しいドメイン名を追加するシナリオがあります。  使用するドメインの例は、we-retail.adobe.com です。  まず、既存の設定ファイルを新しい設定ファイルにコピーし、そこで変更を追加します。

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

既存の aem_publish.vhost ファイルをコピーしました。これは、既存の aem_publish.vhost ファイルに必要なものが既に存在し、既に強力なスタートを作り直したくないからです。  次に、新しい weretail.vhost ファイルを編集し、必要な変更を行います。

前：

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

後：

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

これで、 `ServerName` および `ServerAlias` 新しいドメイン名を照合し、他のパンくずヘッダーを更新する場合。  次に、新しいファイルを有効にして、Apache が新しいファイルを使用することを知らせるようにします。

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Apache Web サーバーは、ドメインがトラフィックを生成する必要があることを認識していますが、Dispatcher モジュールに対して、受け入れる新しいドメイン名を持つように通知する必要があります。  まず、新しい `*_vhost.any` ファイル `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` そのファイル内に、受け入れたいドメイン名を入れます。

```
"we-retail.adobe.com"
```

次に、新しい vhost エントリファイルを使用する新しいファームファイルを作成する必要があります。まず、新しいファイルに強力な開始ファイルをコピーします。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

このファームファイルに加える必要のある変更を表示します

前：

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

後：

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

これで、ファーム名を更新し、で使用するをインクルードしました。 `/virtualhosts` ファーム設定のセクション。  この新しいファームファイルを有効にして、実行中の設定で使用できるようにする必要があります。

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Web サーバーサービスを再読み込みし、新しいドメインを使用します。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

変更したのは、ベースライン設定ファイルに付属の既存のインクルードとコードを変更して活用するために必要な部分だけです。  必要な要素を説明するだけです。  より簡単になり、より少ないコードを維持できます
</div>

[次へ —> Dispatcher ヘルスチェック](./health-check.md)