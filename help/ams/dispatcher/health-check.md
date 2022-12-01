---
title: AMS Dispatcher ヘルスチェック
description: AMS は、AEMが正常かどうか、および公共のトラフィックに対してサービスを提供する必要があるかどうかを確認するためにクラウドロードバランサーが実行するヘルスチェック cgi-bin スクリプトを提供します。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# AMS Dispatcher ヘルスチェック

[目次](./overview.md)

[&lt; — 前：読み取り専用ファイル](./immutable-files.md)

AMS ベースラインがインストールされた Dispatcher がある場合、いくつかのフリービーが付属します。  これらの機能の 1 つに、ヘルスチェックスクリプトのセットがあります。
これらのスクリプトを使用すると、AEMスタックの前面にあるロードバランサーが、どの脚が正常かを知り、それらを保守できます。

![トラフィックフローを示すアニメーションGIF](assets/load-balancer-healthcheck/health-check.gif "ヘルスチェックの手順")

## 基本的なロードバランサーヘルスチェック

顧客トラフィックがインターネットを通じてAEMインスタンスに到達すると、ロードバランサーを経由します

![画像は、ロードバランサーを介したインターネットから AEM へのトラフィックフローを示します](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

ロードバランサーを通じて受け取る各リクエストは、各インスタンスに対してラウンドロビンを実行します。  ロードバランサーには、正常なホストにトラフィックが送信されていることを確認するヘルスチェックメカニズムが組み込まれています。

通常、デフォルトのチェックは、ロードバランサーでターゲットとなるサーバーがポートトラフィックをリッスンしているかどうかを確認するポートチェックです（TCP 80 および 443）。

> `Note:` これは機能しますが、AEMが正常な場合は、実際の測定値はありません。  Dispatcher（Apache Web サーバー）が起動して実行されている場合にのみテストされます。

## AMS ヘルスチェック

正常でないAEMインスタンスを前面とする健全な Dispatcher にトラフィックを送信しないように、AMS では、Dispatcher だけでなく、脚の状態を評価するいくつかの追加機能を作成しました。

![画像は、ヘルスチェックが機能するための様々な部分を示します](assets/load-balancer-healthcheck/health-check-pieces.png "ヒールチェックピース")

ヘルスチェックは、次の要素で構成されます
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

各作品が何をするべきか、その重要性について説明します

### AEM Package

AEMが機能しているかどうかを示すには、基本的なページのコンパイルをおこない、ページを提供する必要があります。  Adobe Managed Services は、テストページを含む基本パッケージを作成しました。  ページは、リポジトリが起動し、リソースとページテンプレートがレンダリングできることをテストします。

![画像は、CRX パッケージマネージャーに AMS パッケージを示しています](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")

これがページです。  インストールのリポジトリ ID が表示されます

![画像は、AMS Regent ページを示します](assets/load-balancer-healthcheck/health-check-page.png "ヘルスチェックページ")

> `Note:` ページがキャッシュできないことを確認します。  キャッシュされたページが返されるたびに、実際のステータスはチェックされません。

これは、AEMが起動および実行中であることを確認するためにテストできるライトウェイトエンドポイントです。

### ロードバランサーの設定

ロードバランサーを、ポートチェックを使用する代わりに CGI-BIN エンドポイントを指すように設定します。

![画像は、AWSロードバランサーのヘルスチェック設定を示します](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![画像は、Azure ロードバランサーのヘルスチェック構成を示しています](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache Health Check 仮想ホスト

#### CGI-BIN 仮想ホスト `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

これは `<VirtualHost>` CGI-Bin ファイルを実行できるようにする Apache 設定ファイル。

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin ファイルは、実行可能なスクリプトです。  これは脆弱な攻撃ベクトルである可能性があり、AMS が使用するこれらのスクリプトは、テスト対象のロードバランサーからのみ公開アクセスできるわけではありません。


#### 非正常なメンテナンス仮想ホスト

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

これらのファイルの名前は、 `000_` を、目的上のプレフィックスとして使用します。  ライブサイトと同じドメイン名を使用するように意図的に設定されています。  目的は、ヘルスチェックがAEMのバックエンドの 1 つに問題があることを検出した場合に、このファイルを有効にすることです。  次に、ページのない 503 HTTP 応答コードの代わりに、エラーページを表示します。  普通の人から交通を盗む `.vhost` ファイルを読み込む前に読み込まれるので `.vhost` 同じファイルを共有中にファイルを作成 `ServerName` または `ServerAlias`.  その結果、特定のドメイン用のページが、通常のトラフィックフローが通るデフォルトのホストではなく、異常なホストに送られるようになります。

ヘルスチェックスクリプトを実行すると、現在のヘルスステータスがログアウトされます。  1 分に 1 回、サーバー上で cron ジョブが実行され、ログ内の不健全なエントリを探します。  オーサーAEMインスタンスが正常でないことを検出した場合は、次の symlink が有効になります。

ログエントリ：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron がエラーを取得し、次の操作に対応します。

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

作成者または公開済みのサイトがこのエラーページを読み込めるかどうかを制御するには、 `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有効なオプション：
- 作成者
   - これはデフォルトのオプションです。
   - これにより、作成者が正常でない場合に備えてメンテナンスページが作成されます
- publish
   - このオプションを選択すると、パブリッシャーが正常でない場合に、パブリッシャーのメンテナンスページが表示されます
- all
   - このオプションを選択すると、作成者、発行者、またはその両方が不健康になった場合に、メンテナンスページが表示されます
- なし
   - このオプションを選択すると、ヘルスチェックのこの機能がスキップされます

を見ると `VirtualHost` これらの設定では、有効化時に表示される各リクエストのエラーページと同じドキュメントが読み込まれます。

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
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

応答コードは、引き続き `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

空白のページの代わりに、代わりにこのページが表示されます。

![画像はデフォルトのメンテナンスページを示します](assets/load-balancer-healthcheck/unhealthy-page.png "異常ページ")

### CGI-Bin スクリプト

CSE がロードバランサー設定で設定できる 5 つの異なるスクリプトがあり、Dispatcher をロードバランサーから引き出す際の動作や条件を変更します。

#### /bin/checkauthor

このスクリプトを使用すると、前面にあるインスタンスを確認してログに記録しますが、エラーが返されるのは、 `author` AEMインスタンスが異常です

> `Note:` パブリッシュAEMインスタンスのヘルシー状態が悪かった場合、Dispatcher はサービスを維持し、オーサーAEMインスタンスにトラフィックを送信できるようになります

#### /bin/checkpublish （デフォルト）

このスクリプトを使用すると、前面にあるインスタンスを確認してログに記録しますが、エラーが返されるのは、 `publish` AEMインスタンスが異常です

> `Note:` オーサーAEMインスタンスの正常性が低かった場合、Dispatcher はサービスを維持して、トラフィックがパブリッシュAEMインスタンスに送信されるのを許可することに注意してください

#### /bin/checkeether

このスクリプトを使用すると、前面にあるインスタンスを確認してログに記録しますが、エラーが返されるのは、 `author` または `publisher` AEMインスタンスが異常です

> `Note:` パブリッシュAEMインスタンスまたはオーサーAEMインスタンスのどちらが正常でなかった場合、Dispatcher はサービスを停止することに注意してください。  つまり、そのうちの 1 つが正常な場合は、トラフィックも受信されません

#### /bin/checkboth

このスクリプトを使用すると、前面にあるインスタンスを確認してログに記録しますが、エラーが返されるのは、 `author` そして `publisher` AEMインスタンスが異常です

> `Note:` パブリッシュAEMインスタンスまたはオーサーAEMインスタンスのヘルシーが失われた場合、Dispatcher はサービスを停止しないことに注意してください。  つまり、そのうちの 1 つが異常な場合でも、引き続きトラフィックを受け取り、リソースを要求する人にエラーを与えます。

#### /bin/healthy

このスクリプトを使用すると、前面にあるインスタンスを確認してログに記録しますが、AEMがエラーを返しているかどうかに関係なく、正常に返されるだけです。

> `Note:` このスクリプトは、ヘルスチェックが必要に応じて機能せず、オーバーライドでAEMインスタンスをロードバランサーに保持できる場合に使用します。

[次へ —> GIT Symlinks](./git-symlinks.md)