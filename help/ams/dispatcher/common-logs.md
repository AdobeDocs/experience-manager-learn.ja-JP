---
title: AEM Dispatcher の共通ログ
description: Dispatcher からの一般的なログエントリと、その意味と対処方法について説明します。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# 共通ログ

[目次](./overview.md)

[&lt; — 前：バニティ URL](./disp-vanity-url.md)

## 概要

ドキュメントでは、表示される一般的なログエントリとその意味、およびそれらの処理方法について説明します。

## GLOB 警告

サンプルログエントリ：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Dispatcher モジュール 4.2.x 以降では、フィルターファイルで次のタイプの一致を使用することはお勧めしませんでした。

```
/0041 { /type "allow" /glob "* *.css *"   }
```

代わりに、次のような新しい構文を使用する方が良いです。

```
/0041 { /type "allow" /url "*.css" }
```

または、ワイルドカードのマッチャーで一致しない方が、さらに適しています。

```
/0041 { /type "allow" /extension "css" }
```

推奨される方法のいずれかを実行すると、ログからそのエラーメッセージが削除されます。

## 却下をフィルター


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
ログレベルが低すぎる場合に却下が発生しても、これらのエントリが常に表示されるわけではありません。 これを Info または debug に設定すると、フィルターが要求を拒否しているかどうかを確認できます。
</div>

サンプルログエントリ：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

または:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>注意 ：</b>

Dispatcher のルールが、その要求をフィルターして除外するように設定されていることを理解します。 この場合、訪問を試みたページは目的で拒否されたので、この操作は何も実行しません。
</div>

ログが次のエントリのような場合：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

これは私たちのデザインファイルを知らせています `.eot` はブロックされているので、修正します。
そのため、次の行を追加して、 `.eot` ファイル経由

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

これにより、ファイルの通過と、ログの記録が停止します。
何がフィルタリングされているかを確認するには、ログファイルで次のコマンドを実行します。

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## レンダリングからのタイムアウト

ソケットタイムアウトのサンプルログエントリ：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

これは、ファームの renders セクションに間違った IP アドレスが設定されている場合に発生します。 AEMインスタンスが応答またはリッスンを停止し、Dispatcher がリーチできない。

ファイアウォールルールを確認し、AEMインスタンスが正常に動作していることを確認します。

ゲートウェイタイムアウトのサンプルログエントリ：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

これは、AEMインスタンスが到達可能なオープンソケットを持ち、応答でタイムアウトしたことを意味します。 これは、AEMインスタンスが遅すぎるか、正常でなかったため、Dispatcher がファームのレンダーセクションで設定されたタイムアウト設定に達したことを意味します。 タイムアウト設定を増やすか、AEMインスタンスを正常にします。

## キャッシュレベル

サンプルログエントリ：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

つまり、レンダーレベルからの取得とキャッシュからの取得が測定されます。 キャッシュから 80%以上ヒットしたい場合は、ヘルプに従う必要があります [ここ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

この数をできるだけ多くする。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
ファームファイルにキャッシュ設定がある場合でも、フラッシュする頻度が多すぎる、または過度に過剰で、キャッシュヒット率が低くなる可能性があります
</div>

## ディレクトリがありません

サンプルログエントリ：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

これは、通常、キャッシュの消去が同時に発生している間にアイテムが取得される際に表示されます。

このディレクトリまたはドキュメントルートディレクトリに対する権限が正しくないか、フォルダー上の誤った SELinux ファイルコンテキストがあるので、Apache が新しい必要なサブディレクトリを作成するのを拒否しています。

権限の問題については、ドキュメントルートの権限を調べ、次のようになっていることを確認してください。

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## バニティ URL が見つかりません

サンプルログエントリ：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

このエラーは、動的な自動フィルターを使用するように Dispatcher を設定したが、AEMレンダラーにパッケージをインストールして設定が完了していない場合に発生します。

この問題を修正するには、AEMインスタンスにバニティー URL 機能パックをインストールし、匿名ユーザーがその機能パックを準備できるようにしてください。 詳細 [ここ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

動作するバニティー URL が次のように設定されます。

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## ファームが見つかりません

サンプルログエントリ：

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

このエラーは、で使用可能なすべてのファームファイルからを示します。 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 一致するエントリを `/virtualhost` 」セクションに入力します。

ファームファイルは、リクエストが含まれるドメイン名またはパスに基づいてトラフィックを一致させます。 glob 照合を使用し、一致しない場合は、ファームを正しく設定していないか、ファームへのエントリを入力しているか、エントリが完全に見つからない状態になっています。 ファームがどのエントリとも一致しない場合、最終的には、含まれるファームファイルのスタックに含まれる最後のファームにデフォルト設定されます。 この例では、次のようになりました。 `999_ams_publish_farm.any` publishfarm の汎用名です。

サンプルのファームファイルを次に示します `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 関連するパーツを強調するために縮小されました。

## 提供元の項目

サンプルログエントリ：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

コンテンツのGEThttp メソッドを使用してページを取得しました `/content/we-retail/us/en.html` それには24034ミリ秒かかりました 我々が注目したかった部分は、最後にある。 `publishfarm/0`. これが `publishfarm`. 要求はレンダリング 0 から取得されました。 つまり、このページはAEMからリクエストされ、キャッシュされる必要がありました。 次に、このページを再度リクエストして、ログに何が起こるかを確認しましょう。

[次へ —> 読み取り専用ファイル](./immutable-files.md)