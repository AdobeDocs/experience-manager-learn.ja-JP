---
title: AMS Dispatcher の基本的なファイルレイアウト
description: 基本的な Apache および Dispatcher ファイルレイアウトについて
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 354
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: ht
source-wordcount: '1130'
ht-degree: 100%

---

# 基本的なファイルレイアウト

[目次](./overview.md)

[&lt;- 前：「Dispatcher」とは](./what-is-the-dispatcher.md)

このドキュメントでは、AMS 標準設定ファイルセットと、この設定標準の背後にある考え方を説明します

## デフォルトの Enterprise Linux フォルダー構造

AMS では、基本インストールは、基本オペレーティングシステムとして Enterprise Linux を使用します。Apache web サーバーをインストールすると、デフォルトのインストールファイルが設定されます。yum リポジトリが提供する基本 RPM をインストールするとインストールされるデフォルトのファイルを次に示します

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

インストールの設計／構造に従って作業を行うと、次の利点が得られます。

- 予測可能なレイアウトのサポートが容易になります
- 過去に Enterprise Linux HTTPD のインストールを行った人は誰でも自動的に理解できます
- 競合や手動の調整なしに、オペレーティングシステムで完全にサポートされるパッチ適用サイクルを許可します
- 誤ってラベル付けされたファイルコンテキストの SELinux の違反を回避します

>[!BEGINSHADEBOX 「メモ」]

Adobe Managed Services サーバーの画像には、通常、オペレーティングシステムの小さなルートドライブが付いています。通常 `/mnt` にマウントされる別のボリュームにデータを格納します。
その後、次のデフォルトディレクトリのデフォルトの代わりに、そのボリュームを使用します。

`DocumentRoot`
- デフォルト：`/var/www/html`
- AMS：`/mnt/var/www/html`

`Log Directory`
- デフォルト：`/var/log/httpd`
- AMS：`/mnt/var/log/httpd`

古いディレクトリと新しいディレクトリは、混乱を避けるために元のマウントポイントに再びマッピングされることに注意してください。
別のボリュームを使用することは必須ではありませんが、特筆すべきことです

>[!ENDSHADEBOX]

## AMS アドオン

AMS は、Apache web サーバーのベースインストールに追加されます。

### ドキュメントルート

AMS デフォルトのドキュメントルート：
- オーサー：
   - `/mnt/var/www/author/`
- パブリッシュ：
   - `/mnt/var/www/html/`
- キャッチオールおよびヘルスチェックのメンテナンス
   - `/mnt/var/www/default/`

### ステージングおよび有効な VirtualHost ディレクトリ

次のディレクトリを使用すると、ファイルで作業できるステージング領域を持つ設定ファイルを構築し、準備が整ったときにのみ有効にできます。
- `/etc/httpd/conf.d/available_vhosts/`
   - このフォルダーは、`.vhost` と呼ばれるすべての VirtualHost / ファイルをホストします
- `/etc/httpd/conf.d/enabled_vhosts/`
   - `available_vhosts` フォルダー内の `.vhost` ファイルを使用する準備が整ったら、`enabled_vhosts` ディレクトリに相対パスでシンボリックリンクを設定します

### 追加の `conf.d` ディレクトリ

Apache の設定で共通する追加的な部分があり、ファイルをきれいに分離し、すべてのファイルを 1 つのディレクトリに格納しないようにするために、サブディレクトリを作成しました。

#### ディレクトリの書き換え

このディレクトリには、Apache web サーバーの [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) モジュールに関わる典型的な RewriteRulesyntax を含むすべての `_rewrite.rules` ファイルを格納できます

- `/etc/httpd/conf.d/rewrites/`

#### 許可リストディレクトリ

このディレクトリは、Apache web サーバーの[アクセス制御](https://httpd.apache.org/docs/2.4/howto/access.html)に関与する典型的な `IP Allow` または `Require IP`syntax を含め、作成されるすべての `_whitelist.rules` ファイルを格納できます

- `/etc/httpd/conf.d/whitelists/`

#### 変数ディレクトリ

このディレクトリは、設定ファイルで使用する変数を含む、作成したすべての `.vars` ファイルを格納できます

- `/etc/httpd/conf.d/variables/`

### Dispatcher モジュール固有の設定ディレクトリ

Apache web サーバーは非常に拡張可能で、モジュールに多数の設定ファイルが含まれている場合は、デフォルトの設定ディレクトリを使用する代わりに、インストールの基本ディレクトリの下に独自の設定ディレクトリを作成することがベストプラクティスです。

ベストプラクティスに従って独自の設定ディレクトリを作成しました

#### モジュール設定ファイルディレクトリ

- `/etc/httpd/conf.dispatcher.d/`

#### ステージングおよび有効化されたファーム

次のディレクトリを使用すると、ファイルで作業できるステージング領域を持つ設定ファイルを構築し、準備が整ったときにのみ有効にできます。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - このフォルダーには、`_farm.any` と呼ばれる `/myfarm {` ファイルがすべてホストされます
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - available_farms フォルダー内にあるファームファイルを使用する準備が整ったら、enabled_farms ディレクトリへの相対パスを使用して、そのファイルのシンボリックリンクを作成します

### 追加の `conf.dispatcher.d` ディレクトリ

Dispatcher ファームファイル設定のサブセクションである追加の要素があります。サブディレクトリを作成して、それらのファイルをきれいに分離し、すべてのファイルを 1 つのディレクトリに格納しないようにしました。

#### キャッシュディレクトリ

このディレクトリには、作成したすべての `_cache.any` および `_invalidate.any` ファイルが含まれています。このファイルには、AEM から取得したキャッシュ要素をモジュールで処理する方法に関するルールと、無効化ルールの構文が含まれています。この節の詳細については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#configuring-the-dispatcher-cache-cache)を参照してください。

- `/etc/httpd/conf.dispatcher.d/cache/`

#### クライアントヘッダーディレクトリ

このディレクトリには、作成したすべての `_clientheaders.any` ファイルを含めることができます。これらのファイルは、リクエストが着信したときに AEM に渡すクライアントヘッダーのリストを含んでいます。この節の詳細については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja)を参照してください。

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### フィルターディレクトリ

このディレクトリには、作成したすべての `_filters.any` ファイルを含めることができます。これらのファイルは、Dispatcher を通過するトラフィックが AEM に到達するのをブロックまたは許可するすべてのフィルタールールを含んでいます。

- `/etc/httpd/conf.dispatcher.d/filters/`

#### レンダーディレクトリ

このディレクトリには、作成したすべての `_renders.any` ファイルを含めることができます。これらのファイルには、Dispatcher で使用するコンテンツを提供する各バックエンドサーバーへの接続の詳細が含まれています。

- `/etc/httpd/conf.dispatcher.d/renders/`

#### vhosts ディレクトリ

このディレクトリには、作成したすべての `_vhosts.any` ファイルを含めることができます。これらのファイルは、特定のファームから特定のバックエンドサーバーまでに一致するドメイン名とパスのリストを含んでいます。

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完全なフォルダー構造

AMS では、名前空間の問題、競合や混乱を避けるために、カスタムファイル拡張子を使用して各ファイルを構造化しています。

AMS のデフォルトデプロイメントの標準ファイルセットの例を次に示します。

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## 理想的な状態の維持

Enterprise Linux には、Apache Webserver Package（httpd）のパッチ適用サイクルがあります。

インストール済みのデフォルトファイルが少なければ少ないほど、変更する方がよくなります。この理由は、パッチが適用されたセキュリティ修正または設定改善が RPM または Yum コマンドを介して適用された場合、変更されたファイルの上に修正が適用されないからです。

代わりに、`.rpmnew` ファイルがオリジナルの横に作成されます。つまり、必要な変更が一部失われ、設定フォルダーにガベージが作成されている可能性があるということです。

すなわち、アップデートのインストール時の RPM は、`unaltered` 状態にある場合は `httpd.conf` を参照し、ファイルを&#x200B;*置換*&#x200B;し、重要なアップデートを取得します。  `httpd.conf` が `altered` だった場合、ファイルは&#x200B;*置き換えられず*、代わりに `httpd.conf.rpmnew` という参照ファイルが作成され、必要な修正の多くが、サービスの起動時には適用されないその参照ファイルに含まれます。

Enterprise Linux は、このユースケースをより適切に処理するために適切に設定されていました。設定されたデフォルトを拡張または上書きできる領域が提供されます。httpd のベースインストール内には、ファイル `/etc/httpd/conf/httpd.conf` があります。これには、次のような構文が含まれます。

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

考え方としては、`.conf` というファイル拡張子を持つ新しいファイルを `/etc/httpd/conf.d/` および `/etc/httpd/conf.modules.d/` ディレクトリに追加する際にはモジュールと設定を拡張することを Apache が求めているということです。

Dispatcher モジュールを Apache に追加する際の最適な例として、モジュール `.so` ファイルを ` /etc/httpd/modules/` に作成したあと、内容を含んだファイルを `/etc/httpd/conf.modules.d/02-dispatcher.conf` に追加してモジュール `.so` ファイルを読み込むことで、そのモジュールファイルを組み込みます。

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>Apache から提供された既存のファイルは一切変更していません。代わりに、目的のディレクトリに独自のファイルを追加しました。

これで、独自のファイル <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 内で独自のモジュールを使用するようになりました。このファイルにより独自のファイルモジュールが初期化され、初期モジュール固有の設定ファイルが読み込まれます。

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

ここでも、ファイルとモジュールが追加されましたが、元のファイルは変更されていません。これにより必要な機能が提供され、必要なパッチ修正の欠落から保護されるだけでなく、パッケージのアップグレードごとに最高レベルの互換性が維持されます。

[次へ／設定ファイルの説明](./explanation-config-files.md)
