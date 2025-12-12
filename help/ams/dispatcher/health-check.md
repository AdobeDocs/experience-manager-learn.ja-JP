---
title: AMS Dispatcher ヘルスチェック
description: AMS は、クラウドロードバランサーが実行するヘルスチェック cgi-bin スクリプトを提供して、AEM が正常かどうか、パブリックトラフィックに対してサービスを維持する必要があるかどうかを確認します。
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
duration: 247
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 100%

---

# AMS Dispatcher ヘルスチェック

[目次](./overview.md)

[&lt;- 前：読み取り専用ファイル](./immutable-files.md)

AMS ベースラインがインストールされた Dispatcher をお持ちの場合、いくつかの特典が付いています。これらの機能の 1 つに、ヘルスチェックスクリプトのセットがあります。
これらのスクリプトにより、AEM スタックの前に置かれたロードバランサーは、正常なレッグを認識し、サービスを維持することができます。

![トラフィックフローを示すアニメーション GIF](assets/load-balancer-healthcheck/health-check.gif "ヘルスチェックの手順")

## 基本的なロードバランサーヘルスチェック

顧客トラフィックがインターネットを経由して AEM インスタンスに到達する場合、トラフィックはロードバランサーを通過します。

![インターネットからロードバランサーを経由する AEM へのトラフィックフローを示す画像](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

ロードバランサーを経由して受け取る各リクエストは、各インスタンスに対してラウンドロビンを実行します。  ロードバランサーには、正常なホストにトラフィックが送信されていることを確認するヘルスチェックメカニズムが組み込まれています。

デフォルトのチェックは通常、ロードバランサーでターゲットとなるサーバーが、トラフィックが入ってくるポート（すなわち TCP 80 と 443）をリッスンしているかどうかを確認するためのポートチェックです。

> `Note:` これは機能しますが、AEM が正常であるかどうか実際に測定するものではありません。  Dispatcher（Apache web サーバー）が起動して実行されている場合にのみ、テストが実行されます。

## AMS ヘルスチェック

正常でない AEM インスタンスの前にある正常な Dispatcher にトラフィックが送信されるのを避けるために、AMS は Dispatcher だけでなく、レッグの正常性を評価するいくつかの余分を作成しました。

![ヘルスチェックを機能させるための様々な部分を示す画像](assets/load-balancer-healthcheck/health-check-pieces.png "healt-check-pieces")

ヘルスチェックは、以下の要素で構成されています
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

各要素の設定内容とその重要性について説明します

### AEM パッケージ

AEM が機能しているかどうかを示すには、基本的なページのコンパイルを実行し、ページを提供する必要があります。Adobe Managed Services は、テストページを含む基本パッケージを作成しました。  このページは、リポジトリが起動し、リソースとページテンプレートがレンダリングできることをテストします。

![CRX パッケージマネージャー内の AMS パッケージを示す画像](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")

このページです。  インストールのリポジトリ ID が表示されます

![AMS Regent ページを示している画像](assets/load-balancer-healthcheck/health-check-page.png "health-check-page")

> `Note:` ページがキャッシュできないことを確認します。  毎回キャッシュされたページを返すだけの場合は、実際のステータスはチェックされません。

これは、AEM が起動され、実行中であることを確認するためにテストできる、軽量のエンドポイントです。

### ロードバランサーの設定

ポートチェックを使用する代わりに CGI-BIN エンドポイントを指すように、ロードバランサーを設定します。

![AWS ロードバランサーのヘルスチェック設定を示す画像](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![Azure ロードバランサーのヘルスチェック設定を示す画像](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache ヘルスチェック仮想ホスト

#### CGI-BIN 仮想ホスト `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

これは CGI-Bin ファイルを実行できるようにする `<VirtualHost>` Apache 設定ファイルです。

```
Listen 81
<VirtualHost *:81>
    ServerName "health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin ファイルは、実行可能なスクリプトです。これは脆弱な攻撃経路となる可能性があり、AMS が使用するこれらのスクリプトは一般には公開されておらず、ロードバランサーがテストするためにのみ利用可能です。


#### 非正常なメンテナンス仮想ホスト

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

これらのファイルには、意図的に接頭辞として `000_` という名前が付けられています。ライブサイトと同じドメイン名を使用するように、意図的に設定されています。  命名の目的は、AEM バックエンドの 1 つに問題があることがヘルスチェックで検出された場合に、このファイルを有効にすることです。次に、ページのない 503 HTTP 応答コードのみの代わりに、エラーページを表示します。同じ `ServerName` または `ServerAlias` を共有しながら、その `.vhost` ファイルの前に読み込まれるため、通常の `.vhost` ファイルからトラフィックを盗むことになります。その結果、特定のドメイン宛てのページが、通常のトラフィックフローが通過するデフォルトの vhost ではなく、異常な vhost に移動します。

ヘルスチェックスクリプトを実行すると、現在のヘルスステータスがログアウトされます。  1 分に 1 回、サーバー上で cron ジョブが実行され、ログ内の非正常なエントリを探します。  オーサー AEM インスタンスが正常ではないことが検出されると、シンボリックリンクが有効になります。

ログエントリ：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron がエラーを検出して反応します。

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

`/var/www/cgi-bin/health_check.conf` でリロードモード設定を指定すると、作成者または公開済みサイトがこのエラーページを読み込めるかどうかを制御できます。

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有効なオプション：

- author
   - これはデフォルトのオプションです。
   - これにより、正常でない場合にオーサー用のメンテナンスページが表示されます。
- publish
   - このオプションを選択すると、パブリッシュが正常でない場合に、メンテナンスページが表示されます
- all
   - このオプションを選択すると、オーサー、パブリッシュ、またはその両方が正常でなくなった場合に、メンテナンスページが表示されます
- なし
   - このオプションを選択すると、ヘルスチェックのこの機能がスキップされます

これらの `VirtualHost` 設定を見ると、有効になっている場合に表示される各リクエストのエラーページと同じドキュメントが読み込まれることがわかります。

```
<VirtualHost *:80>
    ServerName    unhealthyauthor
    ServerAlias    ${AUTHOR_DEFAULT_HOSTNAME}
    ErrorDocument    503 /error.html
    DocumentRoot    /mnt/var/www/default
    <Directory />
        Options FollowSymLinks
        AllowOverride None
    </Directory>
    <Directory "/mnt/var/www/default">
        AllowOverride None
        Require all granted
    </Directory>
    <IfModule mod_headers.c>
        Header always add X-Dispatcher ${DISP_ID}
        Header always add X-Vhost "unhealthy-author"
    </IfModule>
    <IfModule mod_rewrite.c>
        ReWriteEngine   on
        RewriteCond %{REQUEST_URI} !^/error.html$
        RewriteRule ^/* /error.html [R=503,L,NC]
    </IfModule>
</VirtualHost>
```

応答コードは、引き続き `HTTP 503` です

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

空白のページの代わりに、このページが表示されます。

![デフォルトのメンテナンスページを表示する画像](assets/load-balancer-healthcheck/unhealthy-page.png "正常ではないページ")

### CGI-Bin スクリプト

CSE がロードバランサー設定で指定できる異なるスクリプトが 5 つあります。これらのスクリプトで、ロードバランサーから Dispatcher を引き出す際の動作または基準を変更します。

#### /bin/checkauthor

このスクリプトを使用すると、前にあるすべてのインスタンスがチェックされてログに記録されますが、`author` AEM インスタンスが正常でない場合にのみエラーが返されます

> `Note:` パブリッシュ AEM インスタンスが正常でない場合、オーサー AEM インスタンスへのトラフィックのフローを許可するために、Dispatcher はサービスを維持します。

#### /bin/checkpublish（デフォルト）

このスクリプトを使用すると、前にあるすべてのインスタンスがチェックされてログに記録されますが、`publish` AEM インスタンスが正常でない場合にのみエラーが返されます

> `Note:` オーサー AEM インスタンスが正常でない場合、パブリッシュ AEM インスタンスへのトラフィックのフローを許可するために、Dispatcher はサービスを維持します。

#### /bin/checkeither

このスクリプトを使用すると、前にあるすべてのインスタンスがチェックされてログに記録されますが、`author` または `publisher` AEM インスタンスが正常でない場合にのみエラーが返されます。

> `Note:` パブリッシュ AEM インスタンスまたはオーサー AEM インスタンスのいずれかが正常でない場合、Dispatcher はサービスを停止します。つまり、そのうちの 1 つが正常であれば、トラフィックも受信されません

#### /bin/checkboth

このスクリプトを使用すると、前にあるすべてのインスタンスがチェックされてログに記録されますが、`author` および `publisher` AEM インスタンスが正常でない場合にのみエラーが返されます。

> `Note:` パブリッシュ AEM インスタンスまたはオーサー AEM インスタンスが正常でない場合、Dispatcher はサービスを停止しません。つまり、そのうちの 1 つが正常でない場合、引き続きトラフィックを受信し、リソースをリクエストする人物にエラーを与えることになります。

#### /bin/healthy

このスクリプトを使用すると、前にあるすべてのインスタンスがチェックされてログに記録されますが、AEM がエラーを返しているかどうかに関係なく、正常な状態のみが返されます。

> `Note:` このスクリプトは、ヘルスチェックが必要に応じて機能せず、上書きで AEM インスタンスをロードバランサーに保持できる場合に使用します。

[次へ -> Git シンボリックリンク](./git-symlinks.md)
