---
title: AMS Dispatcher 読み取り専用ファイルまたは不変ファイル
description: 一部のファイルが読み取り専用または編集不可の理由と、希望どおりに機能変更を行う方法について
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
duration: 253
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '824'
ht-degree: 100%

---

# AMS の読み取り専用ファイルまたは不変ファイル

[目次](./overview.md)

[&lt;- 前：共通ログ](./common-logs.md)

## 説明

このドキュメントでは、ロックされているファイルと変更されないファイル、および適切な構成設定を行う方法について説明します。

AMS がシステムをプロビジョニングする際に、すべてが機能し、安全になるベースライン設定を展開します。これらは、AMS が確実に機能とセキュリティのベースラインとして保証したいと考えているものです。これを実現するために、一部のファイルは読み取り専用かつ不変としてマークされ、変更が回避されます。

レイアウトを使用しても、動作を変更したり、必要な変更を上書きしたりすることはできません。これらのファイルを変更する代わりに、元のファイルを置き換える独自のファイルをオーバーレイします。

これにより、AMS が最新の修正とセキュリティの強化で Dispatcher にパッチを適用しても、ファイルは変更されないことが保証されます。その後も、改善点のメリットを活用し、必要な変更のみを採用できます。
![ボールが転がっていくボーリングレーン。  ボールには矢印と「あなた」の文字が表示されている。ガターのバンパーが持ち上がっていて、そこに「不変ファイル」と書かれている。](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
上の画像に示されるように、不変ファイルはゲームの邪魔をしていません。これらのファイルは、パフォーマンスが損なわれるのを阻止し、ボールをレーンの上に維持しています。この方法では、次の重要な機能を活用できます。

- カスタマイズを独自の安全なスペースで処理する
- AEMのオーバーレイメソッドと同様のカスタム変更のオーバーレイ 
- AMS 設定のパッチ適用は、カスタマイズを変更せずに実行できる
- 基本インストールとカスタマイズされた設定のテストを同時に行って、カスタマイズやその他の原因（どのファイルか？）で問題が発生しているかどうかを判別する


以下は、Dispatcher と共にデプロイされるファイルの一般的なリストです。

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

どれが不変ファイルかを判断するには、Dispatcher で次のコマンドを実行します。

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

変数を使用すると、設定ファイル自体を変更せずに、機能を変更できます。変数の値を調整することで、設定の特定の要素を調整できます。ファイル `/etc/httpd/conf.d/dispatcher_vhost.conf` からハイライトできる例は以下のとおりです。

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

表示される標準の値ではなく、DispatcherLogLevel ディレクティブに `DISP_LOG_LEVEL` の変数があるのがわかります。コードのこのセクションの上には、変数ファイルに対する Include ステートメントも表示されます。次に、変数ファイル `/etc/httpd/conf.d/variables/ams_default.vars` を確認します。このファイルの内容は以下のとおりです。

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

上記のように、変数 `DISP_LOG_LEVEL` の現在の値は `info` です。これを調整してトレースやデバッグを行ったり、数値やレベルを選択したりできます。現在は、ログレベルを制御するすべての場所が自動的に調整されます。

### オーバーレイメソッド

上位の Include ファイルはカスタマイズの出発点となるため、理解しておく必要があります。 簡単な例から始めるために、この Dispatcher をポイントする新しいドメイン名を追加するシナリオが使います。使用するドメインの例は、we-retail.adobe.com です。まず、既存の設定ファイルを新しい設定ファイルにコピーし、そこで変更を追加します。

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

既存の aem_publish.vhost ファイルをコピーしました。これは、既存の aem_publish.vhost ファイルに必要なものが既に存在し、強力な開始ファイルを作り直したくないからです。次に、新しい weretail.vhost ファイルを編集し、必要な変更を行います。

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

これで、新しいドメイン名と一致するように `ServerName` および `ServerAlias` を更新し、他のパンくずヘッダーも更新しました。次に、新しいファイルを有効にして、Apache に新しいファイルを使用することを知らせます。

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Apache web サーバーは、ドメインがトラフィックを生成する必要があることを認識していますが、Dispatcher モジュールに対して、新しいドメイン名を保持するように通知する必要があります。まず、新しい `*_vhost.any` ファイル `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` を作成し、保持するドメイン名をそのファイル内に追加します。

```
"we-retail.adobe.com"
```

次に、新しい vhost エントリファイルを使用する新しいファームファイルを作成する必要があります。まず、新しいファイルに強力な開始ファイルをコピーします。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

このファームファイルに加える必要のある変更を以下に示します。

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

これで、ファーム名と、ファーム設定の `/virtualhosts` セクションで使用するインクルードが更新されました。この新しいファームファイルを有効にして、実行中の設定で使用できるようにする必要があります。

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

では、ｗeb サーバーサービスを再読み込みし、新しいドメインを使用します。

>[!NOTE]
>
>変更したのは、ベースライン設定ファイルに付属の既存のインクルードとコードを変更して活用するうえで必要な部分だけです。詳しく記述する必要があるのは、変更が必要な要素だけです。作業がより容易になり、メンテナンス対象のコードを減らすことができるようになりました。

[次は -> Dispatcher ヘルスチェック](./health-check.md)
