---
title: AEM Dispatcher 設定での変数の使用と理解
description: 変数を Apache および Dispatcher モジュールの設定ファイルで使用して、変数を次のレベルに引き継ぐ方法を説明します。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# 変数の使用と理解

[目次](./overview.md)

[&lt; — 前：キャッシュについて](./understanding-cache.md)

このドキュメントでは、Apache Web サーバーと Dispatcher モジュール設定ファイルで変数の機能を活用する方法について説明します。

## 変数

Apache は変数をサポートしており、Dispatcher モジュールのバージョン 4.1.9 以降も変数をサポートしています。

これらを活用して、次のような役に立つ作業を行うことができます。

- 環境固有の設定はすべて設定内でインライン化されず、開発環境の設定ファイルが実稼動環境で同じ機能出力で動作するように抽出されていることを確認します。
- AMS で提供される不変ファイルの機能を切り替えてログレベルを変更します。変更することはできません。
- 次のような変数に基づいて、どのインクルードを使用するかを変更 `RUNMODE` および `ENV_TYPE`
- 一致 `DocumentRoot`および `VirtualHost` Apache 設定とモジュール設定の間の DNS 名。

## ベースライン変数の使用

AMS ベースラインファイルは読み取り専用で不変なので、使用する変数を編集することで、オフ/オンを切り替えたり、設定したりできる機能があります。

### ベースライン変数

AMS のデフォルト変数はファイルで宣言されます `/etc/httpd/conf.d/variables/ootb.vars`.  このファイルは編集できませんが、変数に null 値が含まれていないことを確認するために存在します。  これらは、私たちが含む以降に最初に含まれます `/etc/httpd/conf.d/variables/ams_default.vars`.  このファイルを編集して、これらの変数の値を変更したり、同じ変数名と値を独自のファイルに含めたりできます。

ファイルの内容のサンプルを次に示します `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 例 1 - SSL を強制

上記の変数 `AUHOR_FORCE_SSL`または `PUBLISH_FORCE_SSL` は 1 に設定して、http リクエストで受信したエンドユーザーを https にリダイレクトするよう強制する書き換えルールを有効にすることができます

この切り替えを機能させる設定ファイル構文を次に示します。

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

書き換えルールに含まれる内容は、エンドユーザーブラウザーをリダイレクトするコードですが、変数が 1 に設定されている場合、ファイルを使用できるかどうかがわかります

### 例 2 — ログレベル

変数 `DISP_LOG_LEVEL` を使用して、実行中の構成で実際に使用されるログレベルに対して持つものを設定できます。

ams ベースライン設定ファイルに存在する構文の例を次に示します。

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Dispatcher のログレベルを上げる必要がある場合は、 `ams_default.vars` 変数 `DISP_LOG_LEVEL` 希望のレベルに。

値の例は、整数または単語です。

| ログレベル | 整数値 | 単語の値 |
| --- | --- | --- |
| トレース | 4 | trace |
| デバッグ | 3 | debug |
| 情報 | 2 | info |
| 警告 | 1 | 警告 |
| エラー | 0 | エラー |

### 例 3 — ホワイトリスト

変数 `AUTHOR_WHITELIST_ENABLED` および `PUBLISH_WHITELIST_ENABLED` を 1 に設定すると、IP アドレスに基づくエンドユーザートラフィックを許可または禁止するルールを含む書き換えルールをエンゲージできます。  この機能をオンに切り替えるには、ホワイトリストルールファイルの作成と組み合わせて、ファイルを含める必要があります。

変数がホワイトリストファイルのインクルードを有効にする方法の構文例と、ホワイトリストファイルの例をいくつか示します

 `sample.vhost`：

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

 `sample_whitelist.rules`：

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

見ての通り `sample_whitelist.rules` では IP 制限が適用されますが、変数を切り替えると、 `sample.vhost`

## 変数の配置場所

### Web サーバーの起動引数

AMS は、ファイル内の Apache プロセスの起動引数に、サーバー/トポロジ固有の変数を配置します `/etc/sysconfig/httpd`

このファイルには、次に示すように事前に定義された変数があります。

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

これらは変更できるものではありませんが、設定ファイルで活用するのに適しています

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

このファイルは、サービスの起動時にのみ含まれるためです。  変更を受け取るには、サービスを再起動する必要があります。  再読み込みは十分ではなく、代わりに再起動が必要です。
</div>

### 変数ファイル (`.vars`)

コードから提供されるカスタム変数は、に格納する必要があります。 `.vars` ディレクトリ内のファイル `/etc/httpd/conf.d/variables/`

これらのファイルには任意のカスタム変数を含めることができ、以下のサンプルファイルに構文の例を示します

 `/etc/httpd/conf.d/variables/weretail_domains_dev.vars`：

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

 `/etc/httpd/conf.d/variables/weretail_domains_stage.vars`：

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

 `/etc/httpd/conf.d/variables/weretail_domains_prod.vars`：

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

独自の変数ファイルを作成する場合は、そのファイルの内容に応じて名前を付け、マニュアルに記載されている命名規格に従ってください [ここ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  上記の例では、変数ファイルが、設定ファイルで使用する変数として、様々な DNS エントリをホストしています。

## 変数の使用

これで変数を変数ファイル内で定義したので、他の設定ファイル内での変数の適切な使用方法を知りたいと思います。

例を使用します `.vars` 上記のファイルを使用して、適切な使用例を示します。

すべての環境ベースの変数をグローバルに含めたい場合は、ファイルを作成します `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

httpd サービスが起動すると、AMS によって設定された変数が `/etc/sysconfig/httpd` とは、 `ENV_TYPE` および `RUNMODE`

このグローバルな場合 `.conf` ファイルが取り込まれると、内のファイルのインクルード順序が変わるので、早く取り込まれます。 `conf.d` が英数字の読み込み順序である場合、ファイル名の 000 は、ディレクトリ内の他のファイルより先に読み込まれることを保証します。

include 文では、ファイル名に変数も使用します。  これにより、 `ENV_TYPE` および `RUNMODE` 変数。

この `ENV_TYPE` 値は `dev` 次に、使用されるファイルを示します。

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

この `ENV_TYPE` 値は `stage` 次に、使用されるファイルを示します。

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

この `RUNMODE` 値は `preview` 次に、使用されるファイルを示します。

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

そのファイルが含まれると、その中に保存された変数名を使用できます。

アドビの `/etc/httpd/conf.d/available_vhosts/weretail.vhost` file dev でのみ機能する通常の構文を置き換えることができます。

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

開発、ステージ、実稼動環境で機能する変数の機能を使用する新しい構文を使用する場合：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

アドビの `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` file dev でのみ機能する通常の構文を置き換えることができます。

```
"dev.weretail.com" 
"dev.weretail.net"
```

開発、ステージ、実稼動環境で機能する変数の機能を使用する新しい構文を使用する場合：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

これらの変数は、環境ごとに異なるデプロイ済みファイルを使用する必要なく、実行中の設定を個別化するために非常に多くの再利用をおこないます。  基本的に、変数を使用して設定ファイルをテンプレート化し、変数に基づいてファイルをインクルードします。

## 変数値の表示

変数を使用する場合、設定ファイル内の値を確認するために検索が必要になることがあります。  サーバーで次のコマンドを実行して、解決された変数を表示する方法があります。

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

コンパイル済みの Apache 設定での変数の見え方：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

コンパイル済みの Dispatcher 設定での変数の見え方を次に示します。

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

コマンドの出力から、config ファイル内の変数とコンパイル済み出力の違いがわかります。

設定例

 `/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`：

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

次に、コマンドを実行して、コンパイルされた出力を確認します。

コンパイル済みの Apache 設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

コンパイル済みの Dispatcher 設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[次へ —> フラッシュ中](./disp-flushing.md)