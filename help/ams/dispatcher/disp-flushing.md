---
title: AEM Dispatcher のフラッシュ
description: AEMが Dispatcher から古いキャッシュファイルを無効化する方法を理解します。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---


# Dispatcher バニティー URL

[目次](./overview.md)

[&lt; — 前：変数の使用と理解](./variables.md)

このドキュメントでは、フラッシュの発生方法と、キャッシュのフラッシュと無効化を実行するメカニズムについて説明します。


## 仕組み

### 操作の順序

一般的なワークフローは、コンテンツ作成者がページをアクティベートする際に最適です。パブリッシャーは、新しいコンテンツを受け取る際に、次の図に示すように、Dispatcher に対してフラッシュリクエストをトリガーします。
![作成者は、コンテンツをアクティベートし、トリガーパブリッシャーは、Dispatcher にフラッシュする要求を送信します。](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
この一連のイベントは、新規または変更されたアイテムのみをフラッシュすることを強調します。  これにより、変更がパブリッシャーから取得される前にフラッシュが発生する競合状態を避けるために、キャッシュをクリアする前にパブリッシャーがコンテンツを受け取るようになります。

## レプリケーションエージェント

オーサー環境には、何かがアクティブ化されると、そのファイルとそのすべての依存関係をパブリッシャーに送信するトリガーが発行者を指すように設定されたレプリケーションエージェントがあります。

パブリッシャーはファイルを受け取ると、受信時イベントでトリガーを示すように Dispatcher を指すように設定されたレプリケーションエージェントを持ちます。  次に、フラッシュリクエストがシリアル化され、Dispatcher に送信されます。

### 作成者レプリケーションエージェント

次に、設定済みの標準レプリケーションエージェントのスクリーンショットの例を示します
![AEM web ページ/etc/replication.htmlからの標準レプリケーションエージェントのスクリーンショット](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

通常、作成者に対して、コンテンツのレプリケート先のパブリッシャーごとに 1 つまたは 2 つのレプリケーションエージェントが設定されます。

1 つ目は、コンテンツのアクティベーションをプッシュする標準レプリケーションエージェントです。

2 つ目はリバースエージェントです。  これはオプションで、各パブリッシャーのアウトボックスで、リバースレプリケーションアクティビティとしてオーサーに取り込む新しいコンテンツがあるかどうかを確認するように設定されています

### パブリッシャーレプリケーションエージェント

設定済みの標準フラッシュレプリケーションエージェントのスクリーンショットの例を次に示します
![AEM web ページ/etc/replication.htmlからの標準フラッシュレプリケーションエージェントのスクリーンショット](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### 仮想ホストを受け取る DISPATCHER フラッシュレプリケーション

Dispatcher モジュールは、特定のヘッダーを探して、POSTリクエストがAEMレンダーに渡すものか、またはフラッシュリクエストとしてシリアル化され、Dispatcher ハンドラー自体で処理する必要があるかを知るために探します。

次に、これらの値を示す設定ページのスクリーンショットを示します。
![シリアル化の種類が Dispatcher フラッシュと表示されるメインの設定画面の「設定」タブの画像](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

デフォルト設定ページには、 `Serialization Type` as `Dispatcher Flush` エラーレベルを設定します。

![レプリケーションエージェントのトランスポートタブのスクリーンショット。  これは、フラッシュリクエストを投稿する URI を表示します。  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

の `Transport` タブをクリックすると、 `URI` フラッシュ要求を受け取る Dispatcher の IP アドレスを指すように設定されている。  パス `/dispatcher/invalidate.cache` モジュールが、フラッシュであるかどうかを、アクセスログに表示され、フラッシュリクエストであることを知るための明白なエンドポイントであるかどうかを判断する方法ではありません。  の `Extended` 「 」タブでは、Dispatcher モジュールに対するフラッシュリクエストであると見なすために存在する事項を確認します。

![レプリケーションエージェントの「拡張」タブのスクリーンショット。  Dispatcher にフラッシュを指示するために送信されたPOSTリクエストで送信されるヘッダーに注意します。](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

この `HTTP Method` フラッシュリクエストは `GET` 一部の特別なリクエストヘッダーを持つリクエストを次に示します。
- CQ-Action
   - リクエストに基づくAEM変数を使用します。値は通常、 *有効化または削除*
- CQ-Handle
   - リクエストに基づいてAEM変数を使用します。値は通常、フラッシュされた項目のフルパスです ( 例： `/content/dam/logo.jpg`
- CQ-Path
   - リクエストに基づいてAEM変数を使用します。値は通常、フラッシュされる項目の完全パスです（例： ） `/content/dam`
- ホスト
   - ここで、 `Host` 特定の `VirtualHost` Dispatcher Apache Web サーバー (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`) をクリックします。  これは、 `aem_flush.vhost` ファイルの `ServerName` または `ServerAlias`

![オーサーパブリッシュコンテンツからレプリケーションイベントから新しい項目が受け取られたときに、react とトリガーを持つレプリケーションエージェントが受信したことを示す標準レプリケーションエージェントの画面](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

の `Triggers` タブで、使用する切り替え済みトリガーと、その内容をメモします

- `Ignore default`
   - これを有効にすると、ページのアクティベーション時にレプリケーションエージェントがトリガーされなくなります。  これは、オーサーインスタンスがページに変更を加えた場合にフラッシュがトリガーになる問題です。  これはパブリッシャーなので、このタイプのイベントをトリガーしたくはありません。
- `On Receive`
   - 新しいファイルを受け取ったら、フラッシュをトリガーにします。  したがって、作成者が更新されたファイルを送信すると、トリガーを設定し、フラッシュリクエストを Dispatcher に送信します。
- `No Versioning`
   - 新しいファイルを受け取ったので、パブリッシャーが新しいバージョンを生成しないようにするには、これをチェックします。  作成者に頼って、公開者ではなく既存のファイルを置き換えます。

次に、一般的なフラッシュリクエストが `curl` command

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

このフラッシュの例では、 `/content/dam` パスを更新する `.stat` ファイルを含める必要があります。

## この `.stat` ファイル

フラッシュメカニズムは本質的にシンプルで、 `.stat` キャッシュファイルが作成されるドキュメントルートに生成されるファイル。

内部 `.vhost` および `_farm.any` ファイルドキュメントのルートディレクティブを設定して、キャッシュの場所と、エンドユーザーからの要求が来たときにファイルを保存/提供する場所を指定します。

Dispatcher サーバーで次のコマンドを実行すると、 `.stat` ファイル

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

キャッシュに項目があり、Dispatcher モジュールによってフラッシュリクエストが送信されて処理された場合、このファイル構造はどのように見えるかを次の図に示します

![コンテンツと日付が混在した statfiles と、stat レベルで表示された stat ファイル](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### STAT ファイルのレベル

各ディレクトリには、 `.stat` ファイルが存在する。  これは、フラッシュが発生したことを示すインジケータです。  上記の例では、 `statfilelevel` 設定が次のように設定されています： `3` 対応するファーム設定ファイル内にあります。

この `statfilelevel` 設定は、モジュールがトラバースして更新するフォルダーの深さを示します `.stat` ファイル。  .stat ファイルが空の場合は、データスタンプを含むファイル名に過ぎず、手動で作成することもできますが、Dispatcher サーバーのコマンドラインで touch コマンドを実行します。

stat ファイルレベルの設定が大きすぎる場合、各フラッシュリクエストは、stat ファイルに接するディレクトリツリーを走査します。  これは、大きなキャッシュツリーで大きなパフォーマンスヒットとなる可能性があり、Dispatcher の全体的なパフォーマンスに影響を与える可能性があります。

このファイルレベルを低く設定すると、フラッシュリクエストが意図したよりも多くクリアされる可能性があります。  その結果、キャッシュから提供されるリクエストが少なく、キャッシュがより頻繁にチャーンし、パフォーマンスの問題が発生する可能性があります。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

を `statfilelevel` 妥当なレベルで  フォルダー構造を見て、多くのディレクトリをトラバーする必要なく、簡潔なフラッシュを許可するように設定されていることを確認します。   システムのパフォーマンステスト中に、テストを行い、ニーズに合うことを確認します。

言語をサポートするサイトの例が好ましいです。  一般的なコンテンツツリーには次のディレクトリが含まれます

`/content/brand1/en/us/`

この例では、stat ファイルのレベル設定を 4 にします。  これにより、 <b>`us`</b> フォルダー内の言語フォルダーもフラッシュされません。
</div>

### STAT ファイルのタイムスタンプハンドシェイク

コンテンツのリクエストが同じルーチンで発生した場合

1. のタイムスタンプ `.stat` ファイルは、要求されたファイルのタイムスタンプと比較されます
2. この `.stat` ファイルの方がリクエストされたファイルより新しいので、キャッシュされたコンテンツを削除し、AEMから新しいコンテンツを取得してキャッシュします。  その後、コンテンツを提供
3. この `.stat` ファイルがリクエストされたファイルより古い場合は、そのファイルが新しいことを認識し、コンテンツを提供できます。

### キャッシュハンドシェイク — 例 1

上の例では、コンテンツのリクエストが `/content/index.html`

時刻 `index.html` ファイルは2019-11-01 @ 6:21PM です

最も近い `.stat` ファイルは2019-11-01 @ 12:22PM です

上記で読んだ内容を理解すると、インデックスファイルが `.stat` ファイルとファイルは、キャッシュから要求したエンドユーザーに提供されます。

### キャッシュハンドシェイク — 例 2

上の例では、コンテンツのリクエストが `/content/dam/logo.jpg`

時刻 `logo.jpg` ファイルは2019-10-31 @ 1:13PM です

最も近い `.stat` ファイルは2019-11-01 @ 12:22PM です

この例で示すように、ファイルは `.stat` ファイルが削除され、AEMから新しいファイルが取り出されて、キャッシュ内で置き換えられた後、リクエストしたエンドユーザーに提供されます。

## ファームファイル設定

設定オプションの完全なセットについては、ドキュメントを参照してください。 [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache)

キャッシュのフラッシュに関係する、いくつかの要素を強調します。

### フラッシュファーム

鍵は 2 つあります `document root` オーサートラフィックおよびパブリッシャートラフィックのファイルをキャッシュするディレクトリ。  これらのディレクトリを新しいコンテンツで最新の状態に保つには、キャッシュをフラッシュする必要があります。  これらのフラッシュリクエストは、リクエストを拒否したり望ましくない何かを行う可能性のある、通常の顧客トラフィックファーム設定とは無関係です。  代わりに、このタスクに 2 つのフラッシュファームを提供します。

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

これらのファームファイルは、ドキュメントのルートディレクトリをフラッシュする以外は何もしません。

```
/publishflushfarm {  
	/virtualhosts {
		"flush"
	}
	/cache {
		/docroot "${PUBLISH_DOCROOT}"
		/statfileslevel "${DEFAULT_STAT_LEVEL}"
		/rules {
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
		}
		/invalidate {
			/0000 {
				/glob "*"
				/type "allow"
			}
		}
		/allowedClients {
			/0000 {
				/glob "*.*.*.*"
				/type "deny"
			}
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
		}
	}
}
```

### ドキュメントルート

この設定エントリは、ファームファイルの次のセクションにあります。

```
/myfarm { 
    /cache { 
        /docroot
```

Dispatcher をキャッシュディレクトリとして設定および管理するディレクトリを指定します。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
このディレクトリは、Web サーバーが使用するように設定されているドメインの Apache ドキュメントのルート設定と一致する必要があります。

Apache ドキュメントルートのサブフォルダーにある各ファームごとにネストされた docroot フォルダーを持つことは、多くの理由で非常に困難です。
</div>

### Stat Files Level

この設定エントリは、ファームファイルの次のセクションにあります。

```
/myfarm { 
    /cache { 
        /statfileslevel
```

この設定は、深さを測定します `.stat` フラッシュリクエストが送信されたら、ファイルを生成する必要があります。

`/statfileslevel` 次の番号に、 `/var/www/html/` フラッシュ時に次の結果が得られます `/content/dam/brand1/en/us/logo.jpg`

- 0 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
- 1 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 — 次の stat ファイルが作成されます
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

タイムスタンプのハンドシェイクが発生すると、最も近い `.stat` ファイル。

持つ `.stat` ファイルレベル 0 と stat ファイルは次の場所にのみ存在します： `/var/www/html/.stat` ～の下に住む内容を意味する `/var/www/html/content/dam/brand1/en/us/` 最寄りの `.stat` ファイルを作成し、5 つ上のフォルダーを移動して、唯一の `.stat` ファイルの作成日を指定します。  つまり、このレベルの高いレベルで 1 回フラッシュすると、キャッシュされたすべての項目が基本的に無効化されます。
</div>

### 無効化を許可

この設定エントリは、ファームファイルの次のセクションにあります。

```
/myfarm { 
    /cache { 
        /allowedClients {
```

この設定内では、フラッシュリクエストを送信できる IP アドレスのリストを配置します。  フラッシュリクエストが Dispatcher に含まれる場合、信頼された IP からのリクエストが必要です。  この誤った設定が行われているか、信頼されていない IP アドレスからフラッシュリクエストを送信した場合は、ログファイルに次のエラーが表示されます。

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### 無効化ルール

この設定エントリは、ファームファイルの次のセクションにあります。

```
/myfarm { 
    /cache { 
        /invalidate {
```

これらのルールは、通常、フラッシュリクエストで無効化できるファイルを示します。

ページのアクティベーションによって重要なファイルが無効化されるのを防ぐために、無効にするファイルと手動で無効にする必要のあるファイルを指定するルールを適用できます。  次に、html ファイルの無効化のみを許可する設定のサンプルセットを示します。

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## テスト/トラブルシューティング

ページをアクティベートし、ページのアクティベートが成功したことを示す緑色の光が表示されたら、アクティベートしたコンテンツがキャッシュからもフラッシュされます。

ページを更新し、古い情報を確認します。 what!? 緑の光があった?!

フラッシュプロセスを手動でいくつか実行し、何が間違っているのかを把握してみましょう。  パブリッシャーシェルから、curl を使用して次のフラッシュリクエストを実行します。

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

テストフラッシュリクエストの例

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Dispatcher に対するリクエストコマンドを実行したら、ログでの処理と、 `.stat files`.  ログファイルの末尾に、次のエントリが表示され、フラッシュリクエストが Dispatcher モジュールにヒットしたことを確認できます

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

モジュールが取得され、フラッシュリクエストを確認したので、モジュールが `.stat` ファイル。  次のコマンドを実行し、別のフラッシュを実行する際にタイムスタンプの更新を確認します。

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

コマンド出力からわかるように、現在の `.stat` ファイル

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

フラッシュを再度実行した場合は、タイムスタンプの更新を確認してください

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

コンテンツのタイムスタンプをアドビのタイムスタンプと比較しましょう `.stat` ファイルのタイムスタンプ

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

タイムスタンプを見ると、コンテンツは `.stat` キャッシュからファイルを提供するようモジュールに指示するファイル。 `.stat` ファイル。

明らかに、このファイルのタイムスタンプは、「フラッシュ」または置き換えの条件を満たさない更新されたものです。

[次へ —> バニティー URL](./disp-vanity-url.md)