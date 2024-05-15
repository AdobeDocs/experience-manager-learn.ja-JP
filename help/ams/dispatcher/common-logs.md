---
title: AEM Dispatcher の一般的なログ
description: Dispatcher の一般的なログエントリと、その意味および対処方法について説明します。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 100%

---

# 一般的なログ

[目次](./overview.md)

[&lt; - 前：バニティ URL](./disp-vanity-url.md)

## 概要

このドキュメントでは、表示される一般的なログエントリとその意味およびそれらの対処方法について説明します。

## GLOB 警告

サンプルログエントリ：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Dispatcher モジュール 4.2.x 以降では、フィルターファイルで次のタイプの一致を使用することは推奨されなくなりました。

```
/0041 { /type "allow" /glob "* *.css *"   }
```

代わりに、次のような新しい構文を使用する方が良いでしょう。

```
/0041 { /type "allow" /url "*.css" }
```

または、ワイルドカードのマッチャーで一致しなければさらに良いです。

```
/0041 { /type "allow" /extension "css" }
```

推奨される方法のいずれかを実行すると、ログからそのエラーメッセージが削除されます。

## 却下のフィルタリング

>[!NOTE]
>
>設定されたログレベルが低すぎる場合に却下が発生しても、これらのエントリは常に表示されるわけではありません。これを情報またはデバッグに設定すると、フィルターがリクエストを却下しているかどうかを確認できます。

サンプルログエントリ：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

または

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>そのリクエストを除外するように Dispatcher のルールが設定されていることを理解します。 この場合は、訪問しようとしたページはわざと却下されたので、何も実行しません。

ログが次のエントリのような場合：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

これは、デザインファイル `.eot` がブロックされていることを知らせており、これを修正する必要があります。
そのため、フィルターファイルを見て、次の行を追加して、 `.eot` ファイルがブロックされないようにします。

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

これにより、ファイルは許可され、ログに記録されなくなります。
何がフィルターで除外されているかを確認するには、ログファイルに対して次のコマンドを実行します。

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## レンダーからのタイムアウト

ソケットタイムアウトのサンプルログエントリ：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

これは、ファームの renders セクションに間違った IP アドレスが設定されている場合に発生します。 または、AEM インスタンスが応答またはリッスンしなくなり、Dispatcher がリーチできない場合です。

ファイアウォールルールを調べ、AEM インスタンスが正常に動作していることを確認します。

ゲートウェイタイムアウトのサンプルログエントリ：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

これは、AEM インスタンスのソケットが開いており到達可能であったが、応答でタイムアウトしたことを意味します。 これは、AEM インスタンスが遅すぎるか、正常でなかったので、Dispatcher がファームのレンダーセクションで設定されたタイムアウト設定に達したことを意味します。 タイムアウト設定を増やすか、AEM インスタンスを正常にします。

## キャッシュレベル

サンプルログエントリ：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

つまり、レンダーレベルからの取得とキャッシュからの取得が測定され対比されます。 キャッシュから 80%以上ヒットしたい場合は、[こちら](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html?lang=ja)のヘルプに従ってください。

この数をできるだけ大きくします。

>[!NOTE]
>
>すべてをキャッシュするようにファームファイルにキャッシュ設定を行っている場合でも、フラッシュする頻度が多すぎる、またはフラッシュが過剰すぎるので、キャッシュヒット率が低下する場合があります。

## ディレクトリがない

サンプルログエントリ：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

これは、通常、キャッシュの消去が同時に発生している間にアイテムが取得される場合に表示されます。

または、このディレクトリまたはドキュメントルートディレクトリに対する権限が正しくないか、フォルダー上の SELinux ファイルコンテキストが誤っているので、必要な新しいサブディレクトリを Apache が作成するのを拒否しています。

権限の問題については、ドキュメントルートの権限を調べ、次のようになっていることを確認してください。

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## バニティ URL が見つからない

サンプルログエントリ：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

このエラーは、動的な自動フィルターを使用してバニティ URL を許可するように Dispatcher を設定したが、AEM レンダラーにパッケージをインストールしても設定が完了していない場合に発生します。

この問題を修正するには、AEM インスタンスにバニティ URL 機能パックをインストールし、匿名ユーザーがバニティ URL を読み取れるようにしてください。詳細は[こちら](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html?lang=ja)

動作するバニティ URL が次のように設定されます。

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

このエラーは、`/etc/httpd/conf.dispatcher.d/enabled_farms/` で利用可能なすべてのファームファイルから、`/virtualhost` セクションから一致するエントリを見つけることができなかったことを示しています。

ファームファイルは、リクエストが含まれるドメイン名またはパスに基づいてトラフィックを一致させます。 グロブ照合が使用されます。一致しない場合は、ファームを正しく設定していないか、ファームへのエントリを正しく入力していないか、エントリが完全に欠如しています。 ファームがどのエントリとも一致しない場合、最終的には、含まれるファームファイルのスタックに含まれる最後のファームにデフォルト設定されます。 この例では、publishfarm の総称である `999_ams_publish_farm.any` でした。

これは、関連する部分をハイライトするために縮小されたファームファイル `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` の例です。

## 提供元の項目

サンプルログエントリ：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

このページは、コンテンツ `/content/we-retail/us/en.html` の GET HTTP メソッドを介してフェッチされ、24034 ミリ秒かかりました。注目したかった部分は、最後の `publishfarm/0` にあります。`publishfarm` をターゲットにして一致したことがわかります。リクエストはレンダリング 0 からフェッチされました。つまり、このページは AEM からリクエストされ、キャッシュされる必要がありました。 次に、このページを再度リクエストして、ログに何が起こるかを確認しましょう。

[次へ -> 読み取り専用ファイル](./immutable-files.md)
