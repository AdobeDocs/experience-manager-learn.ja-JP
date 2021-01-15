---
title: ログを使用したAEM SDKのデバッグ
description: ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされたAEMアプリケーションでの適切なログ記録に依存します。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# ログを使用したAEM SDKのデバッグ

AEM SDKのログにアクセスすると、AEM SDKのローカルquickstart JARまたはディスパッチャーツールのいずれかが、AEMアプリケーションのデバッグに関する主要なインサイトを提供できます。

## AEMログ

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされたAEMアプリケーションでの適切なログ記録に依存します。 Adobeでは、AEM SDKのローカルクイックスタートとAEMのログ表示をCloud Serviceの開発環境として正しく認識するので、ローカル開発およびAEMをCloud Service開発ログ設定として可能な限り維持することを推奨します。これにより、設定の混乱と再展開が軽減されます。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)は、

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

`error.log`にログインします。

デフォルトのログがローカル開発に不十分な場合、アドホックログはAEM SDKのローカルクイックスタートのLog Support Webコンソール([/system/console/slinglog](http://localhost:4502/system/console/slinglog))を介して設定できますが、Cloud Service開発環境と同じログ設定がAEMで必要な場合以外は、Gitに対して非推奨です。 ただし、ログサポートコンソールを使用した変更は、AEM SDKのローカルクイックスタートリポジトリに直接保持されます。

Javaログ文は、`error.log`ファイル内の表示にすることができます。

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

出力を端末に送る`error.log`を「しっぽを付ける」のは役に立つことが多い。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windowsには、[サードパーティのテールアプリケーション](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)、または[PowershellのGet-Contentコマンド](https://stackoverflow.com/a/46444596/133936)を使用する必要があります。

## Dispatcher ログ

ディスパッチャーログは、`bin/docker_run`の呼び出し時にstdoutに出力されますが、Dockerに格納されている内のでログに直接アクセスできます。

### Dockerコンテナのログへのアクセス{#dispatcher-tools-access-logs}

ディスパッチャーログは、`/etc/httpd/logs`のDockerコンテナで直接アクセスできます。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_`<CONTAINER ID>` inは、 `docker exec -it <CONTAINER ID> /bin/sh` コマンドで指定したターゲットドッカーコンテナIDに置き換える必要があり `docker ps` ます。_


### Dockerログをローカルファイルシステム{#dispatcher-tools-copy-logs}にコピーする

ディスパッチャーログは、`/etc/httpd/logs`のDockerコンテナからローカルファイルシステムにコピーし、お気に入りのログ分析ツールを使用して検査できます。 これはポイント・イン・タイム・コピーであり、ログにリアルタイムの更新を提供しません。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_`<CONTAINER_ID>` inは、 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` コマンドで指定したターゲットドッカーコンテナIDに置き換える必要があり `docker ps` ます。_
