---
title: AMS Dispatcher の基本ファイルレイアウト
description: 基本的な Apache および Dispatcher ファイルレイアウトについて
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1163'
ht-degree: 2%

---


# 基本ファイルレイアウト

[目次](./overview.md)

[&lt; — 前：「Dispatcher」とは](./what-is-the-dispatcher.md)

このドキュメントでは、AMS 標準設定ファイルセットと、この設定標準の背後にある考え方を説明します

## デフォルトの Enterprise Linux フォルダ構造

AMS では、基本インストールは、基本オペレーティングシステムとして Enterprise Linux を使用します。 Apache Web サーバーをインストールすると、デフォルトのインストールファイルが設定されます。 yum リポジトリが提供する基本 RPM をインストールしてインストールするデフォルトのファイルを次に示します

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

インストールの設計/構造に従って作業を行うと、次の利点が得られます。

- 予測可能なレイアウトを容易にサポート
- 過去に Enterprise Linux HTTPD のインストールを行った人にとって、自動的に身近なもの
- 競合や手動の調整を行わずに、オペレーティングシステムで完全にサポートされるパッチ適用サイクルを許可
- SELinux のラベル付けされていないファイルコンテキストの違反を回避

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
Adobe Managed Services サーバーのイメージには、通常、小さなオペレーティングシステムのルートドライブが付いています。  データを別のボリュームに置き、通常は'/mnt'にマウントされます。その後、以下のデフォルトのディレクトリのデフォルトの代わりに、そのボリュームを使用します。

`DocumentRoot`
- デフォルト:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- デフォルト: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

古いディレクトリと新しいディレクトリは、混乱を避けるために元のマウントポイントに再びマッピングされることに注意してください。
別のボリュームを使用することは重要ではありませんが、注意が必要です
</div>

## AMS アドオン

AMS は、Apache Web サーバーのベースインストールに追加されます。

### ドキュメントルート

AMS デフォルトのドキュメントルート：
- オーサー:
   - `/mnt/var/www/author/`
- パブリッシュ:
   - `/mnt/var/www/html/`
- 包括的およびヘルスチェックのメンテナンス
   - `/mnt/var/www/default/`

### VirtualHost ディレクトリのステージングと有効化

次のディレクトリを使用すると、ファイルで作業できるステージング領域を持つ設定ファイルを構築し、準備が整ったときにのみ有効にすることができます。
- `/etc/httpd/conf.d/available_vhosts/`
   - このフォルダーは、VirtualHost /と呼ばれるすべてのファイルをホストします。 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - を使用する準備が整ったら、 `.vhost` ファイルの `available_vhosts` フォルダーは、 `enabled_vhosts` directory

### 追加 `conf.d` ディレクトリ

Apache の設定で共通の部分が追加され、サブディレクトリを作成して、それらのファイルを明確に分離し、すべてのファイルを 1 つのディレクトリに含めないようにしました

#### ディレクトリを書き換え

このディレクトリには、 `_rewrite.rules` Apache Web サーバーに関与する典型的な RewriteRules 構文を含む作成済みのファイル [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) モジュール

- `/etc/httpd/conf.d/rewrites/`

#### ホワイトリストディレクトリ

このディレクトリには、 `_whitelist.rules` 作成するファイルに、 `IP Allow` または `Require IP`Apache Web サーバーを関与させる構文 [アクセス制御](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 変数ディレクトリ

このディレクトリには、 `.vars` 設定ファイルで使用できる変数を含む、作成するファイル

- `/etc/httpd/conf.d/variables/`

### Dispatcher モジュール固有の設定ディレクトリ

Apache Web サーバーは非常に拡張可能で、モジュールに多数の設定ファイルが含まれている場合は、デフォルトの設定ディレクトリをクラスター化する代わりに、インストールベースディレクトリの下に独自の設定ディレクトリを作成することをお勧めします。

ベストプラクティスに従って独自のを作成しました

#### モジュール設定ファイルディレクトリ

- `/etc/httpd/conf.dispatcher.d/`

#### ステージングおよび有効なファーム

次のディレクトリを使用すると、ファイルで作業できるステージング領域を持つ設定ファイルを構築し、準備が整ったときにのみ有効にすることができます。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - このフォルダーは、 `/myfarm {` ファイル名 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - ファームファイルを使用する準備が整ったら、 available_farms フォルダ内で、 enabled_farms ディレクトリへの相対パスを使用してシンボリックリンクを作成します。

### 追加 `conf.dispatcher.d` ディレクトリ

Dispatcher ファームファイル設定のサブセクションである追加の要素があり、サブディレクトリを作成して、それらのファイルを明確に分離し、すべてのファイルを 1 つのディレクトリに含めないようにしました

#### キャッシュディレクトリ

このディレクトリには、 `_cache.any`, `_invalidate.any` 作成するファイルには、AEMから得られるキャッシュ要素の処理方法と無効化ルール構文に関するルールが含まれます。  この節の詳細は、こちらを参照してください。 [ここ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### クライアントヘッダーディレクトリ

このディレクトリには、 `_clientheaders.any` 作成するファイルには、リクエストが送信されたときにAEMに渡すクライアントヘッダーのリストが含まれます。  この節の詳細は、こちらを参照してください。 [ここ](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-http-headers-to-pass-through-clientheaders)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Filters ディレクトリ

このディレクトリには、 `_filters.any` 作成するファイルに、Dispatcher を介したトラフィックがAEMに到達するのをブロックまたは許可するすべてのフィルタールールが含まれている

- `/etc/httpd/conf.dispatcher.d/filters/`

#### レンダリングディレクトリ

このディレクトリには、 `_renders.any` 作成するファイルに、dispatcher が使用する各バックエンドサーバーへの接続の詳細が含まれています。

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts ディレクトリ

このディレクトリには、 `_vhosts.any` 特定のファームに対して特定のバックエンドサーバーに一致するドメイン名とパスのリストを含む、作成するファイル

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## フォルダー構造全体

AMS では、名前空間の問題や競合、混乱を避けるために、カスタムファイル拡張子を持つ各ファイルを構造化しています。

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

## 理想的な状態を維持

Enterprise Linux には、Apache Webserver Package(httpd) のパッチ適用サイクルがあります。

RPM / Yum コマンドを使用して、パッチが適用されたセキュリティ修正や構成の改善が適用された場合、変更されたファイルの上に修正が適用されないため、インストールされたデフォルトファイルの数が少ないほど、変更が適用されます。

代わりに、 `.rpmnew` ファイルがオリジナルの横に表示されます。  これは、必要な変更が一部失われ、設定フォルダーにガベージが作成される可能性があることを意味します。

つまり、アップデートインストール時の RPM は次のようになります。 `httpd.conf` ( `unaltered` ～を述べる *置換* ファイルと、重要な更新情報が入手されます。  この `httpd.conf` が `altered` その後 *置き換えない* ファイルを作成し、代わりに、 `httpd.conf.rpmnew` そして、必要な修正の多くは、サービスの起動時には適用されない、そのファイルに含まれます。

Enterprise Linux は、この使用例をより適切に処理するために適切に設定されていました。  設定した既定値を拡張または上書きできる領域が提供されます。  httpd のベースインストール内では、ファイルが見つかります `/etc/httpd/conf/httpd.conf`に含まれ、その構文は次のようになります。

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Apache では、新しいファイルを `/etc/httpd/conf.d/` および `/etc/httpd/conf.modules.d/` のファイル拡張子を持つディレクトリ `.conf`

Dispatcher モジュールを Apache に追加する場合の最適な例として、モジュールを作成します。 `.so` ～に入る ` /etc/httpd/modules/` 次に、 `/etc/httpd/conf.modules.d/02-dispatcher.conf` と、モジュールを読み込むためのコンテンツ `.so` ファイル

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
Apache が提供した既存のファイルは変更されませんでした。  代わりに、彼らが行くべきディレクトリに私たちを追加した。
</div><br/>

現在は、ファイル内のモジュールを使用しています。 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> モジュールを初期化し、初期モジュール固有の設定ファイルを読み込む

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

ファイルとモジュールが追加されましたが、元のファイルは変更されていません。  これにより、必要な機能が提供され、必要なパッチ修正が見つからなくなるのを防ぐと共に、パッケージの各アップグレードに対する最高レベルの互換性を維持できます。

[次へ —> 構成ファイルの説明](./explanation-config-files.md)