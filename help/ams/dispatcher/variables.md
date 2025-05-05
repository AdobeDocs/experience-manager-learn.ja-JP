---
title: AEM Dispatcher 設定での変数の使用と理解
description: 変数を Apache および Dispatcher モジュールの設定ファイルで使用して、変数を次のレベルに引き継ぐ方法を説明します。
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1089'
ht-degree: 100%

---

# 変数の使用と理解

[目次](./overview.md)

[&lt;- 前へ：キャッシュについて](./understanding-cache.md)

このドキュメントでは、Apache web サーバーと Dispatcher モジュール設定ファイルで変数の機能を活用する方法について説明します。

## 変数

Apache は変数をサポートしていますが、Dispatcher モジュールのバージョン 4.1.9 以降も変数をサポートしています。

これらを活用して、次のような役に立つ作業を行うことができます。

- 環境固有のものはすべて設定内でインライン化されず、開発環境の設定ファイルが実稼動環境で同じ機能出力で動作するように抽出されていることを確認する。
- AMS から提供され、ユーザーが変更できない不変ファイルの機能を切り替えてログレベルを変更する。
- `RUNMODE` および `ENV_TYPE` などの変数に基づいて、どのインクルードを使用するかを変更する。
- Apache 設定とモジュール設定の間の `DocumentRoot`および `VirtualHost` DNS 名と照合する。

## ベースライン変数の使用

AMS ベースラインファイルは読み取り専用で不変なので、使用する変数を編集することで、オフとオンを切り替えたり、設定したりできる機能があります。

### ベースライン変数

AMS のデフォルト変数はファイル `/etc/httpd/conf.d/variables/ootb.vars` で宣言されます 。  このファイルは編集できませんが、変数に null 値が含まれていないことを確認するために存在します。  これらは、含める `/etc/httpd/conf.d/variables/ams_default.vars` の最初と後に含まれます。このファイルを編集して、これらの変数の値を変更したり、同じ変数名と値を独自のファイルに含めたりできます。

ファイル `/etc/httpd/conf.d/variables/ams_default.vars` の内容のサンプルを次に示します。

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 例 1 - SSL を強制

上記の変数 `AUHOR_FORCE_SSL`または `PUBLISH_FORCE_SSL` を 1 に設定して、http リクエストで受信したエンドユーザーを https にリダイレクトするよう強制する書き換えルールを有効にすることができます

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

### 例 2 - ログレベル

変数 `DISP_LOG_LEVEL` を使用して、実行中の構成で実際に使用されるログレベルに対して持つものを設定できます。

ams ベースライン設定ファイルに存在する構文の例を次に示します。

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Dispatcher のログレベルを上げる必要がある場合は、`ams_default.vars` 変数 `DISP_LOG_LEVEL` を希望のレベルに変更します。

値の例は、整数または単語です。

| ログレベル | 整数値 | 単語の値 |
| --- | --- | --- |
| TRACE | 4 | TRACE |
| デバッグ | 3 | debug |
| 情報 | 2 | info |
| 警告 | 1 | 警告 |
| エラー | 0 | エラー |

### 例 3 - 許可リスト

変数 `AUTHOR_WHITELIST_ENABLED` および `PUBLISH_WHITELIST_ENABLED` を 1 に設定すると、IP アドレスに基づくエンドユーザートラフィックを許可または拒否するルールを含む書き換えルールを使用できます。この機能をオンに切り替えるには、許可リストルールファイルの作成と組み合わせて、ファイルを含める必要があります。

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

見ての通り、`sample_whitelist.rules` では IP 制限が適用されますが、変数を切り替えると `sample.vhost` に含めることができます。

## 変数の配置場所

### Web サーバーのスタートアップ引数

AMS は、ファイル `/etc/sysconfig/httpd` 内の Apache プロセスの起動引数に、サーバー／トポロジ固有の変数を配置します 

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

これらは変更できませんが、設定ファイルで使用するのに適しています

>[!NOTE]
>
>これは、このファイルがサービスの起動時にのみ含まれるためです。変更を反映するには、サービスを再起動する必要があります。再読み込みでは十分ではなく、再起動が必要です。

### 変数ファイル（`.vars`）

コードから提供されるカスタム変数は、ディレクトリ `/etc/httpd/conf.d/variables/` 内の `.vars` ファイルに格納する必要があります。

これらのファイルには任意のカスタム変数を含めることができます。以下のサンプルファイルには構文の例が示されています

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

独自の変数ファイルを作成する場合は、内容に応じて名前を付け、マニュアルの[こちら](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html?lang=ja#naming-convention)に記載されている命名基準に従ってください。上記の例では、変数ファイルが設定ファイルで使用する変数として、様々な DNS エントリをホストしています。

## 変数の使用

これで変数を変数ファイル内で定義したので、他の設定ファイル内での変数の適切な使用方法を説明します。

上記の `.vars` ファイルの例により、適切な使用例を説明します。

すべての環境変数をグローバルに取り込む場合は、`/etc/httpd/conf.d/000_load_env_vars.conf` ファイルを作成します。

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

httpd サービスが起動すると AMS が設定した変数を `/etc/sysconfig/httpd` で取り込み、`ENV_TYPE` と `RUNMODE` の変数セットを持つことが分かっています。

 `conf.d` のファイルのインクルード順はアルファベット順なので、このグローバル `.conf` ファイルが取り込まれると、早い段階で取り込まれます。つまり、ファイル名の 000 は、ディレクトリ内の他のファイルよりも先に読み込まれることを保証します。

include ステートメントでは、ファイル名に変数も使用します。これは、変数 `ENV_TYPE` と `RUNMODE` の値によって、実際に読み込むファイルを変えることができます。

`ENV_TYPE` の値が `dev` の場合、使用されるファイルは次のようになります。

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

`ENV_TYPE` の値が `stage` の場合、使用されるファイルは次のようになります。

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

`RUNMODE` の値が `preview` の場合、使用されるファイルは次のようになります。

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

そのファイルがインクルードされると、その中に保存された変数名を使用できます。

`/etc/httpd/conf.d/available_vhosts/weretail.vhost` ファイルでは、開発でのみ動作していた通常の構文を置き換えることができます。

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

変数の機能を利用した新しい構文で、開発、ステージ、実稼動環境に対応します。

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` ファイルでは、開発でのみ動作していた通常の構文を置き換えることができます。

```
"dev.weretail.com" 
"dev.weretail.net"
```

変数の機能を利用した新しい構文で、開発、ステージ、実稼動環境に対応します。

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

これらの変数は、環境ごとに異なるデプロイ済みのファイルを用意することなく、実行設定の個別化のために膨大な再利用が可能です。基本的に、変数を使用して設定ファイルをテンプレート化し、変数に基づいてファイルをインクルードします。

## 変数値の表示

変数を使用する場合、設定ファイル内の値を確認するために検索が必要になることがあります。サーバーで次のコマンドを実行して、解決された変数を表示する方法があります。

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

コンパイル済みの Apache 設定での変数は次のようになります。

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

コンパイル済みの Dispatcher 設定での変数は次のようになります。

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

コマンドの出力から、設定ファイルの変数とコンパイルされた出力の違いを見ることができます。

設定例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`：

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
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

[次へ／フラッシュ](./disp-flushing.md)
