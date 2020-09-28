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
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 3%

---


# ログを使用したAEM SDKのデバッグ

AEM SDKのログにアクセスすると、AEM SDKのローカルquickstart JARまたはディスパッチャーツールのいずれかが、AEMアプリケーションのデバッグに関する主要なインサイトを提供できます。

## AEMログ

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされたAEMアプリケーションでの適切なログ記録に依存します。 Adobeでは、AEM SDKのローカルクイックスタートとAEMのログ表示をCloud Serviceの開発環境として正しく認識するので、ローカル開発およびAEMをCloud Service開発ログ設定として可能な限り維持することを推奨します。これにより、設定の混乱と再展開が軽減されます。

AEM [プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype) は、AEMアプリケーションのJavaパッケージのDEBUGレベルでログを設定し、ローカル開発を行います。ログは、次のURLにあるSling Logger OSGi設定を使用して設定します。

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

ログに記録され `error.log`ます。

デフォルトのログがローカル開発に不十分な場合、アドホックログはAEM SDKのローカルクイックスタートのLog Support Webコンソール([/system/console/slinglog](http://localhost:4502/system/console/slinglog))を介して設定できますが、Cloud Service開発環境と同じログ設定がAEMで必要な場合以外は、Gitに対して非推奨です。 ただし、ログサポートコンソールを使用した変更は、AEM SDKのローカルクイックスタートリポジトリに直接保持されます。

Javaログ文は、次の `error.log` ファイル内の表示にすることができます。

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

出力を端末に流し込む&quot;tail&quot; `error.log` が役に立つ場合が多い。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windowsには、 [サードパーティのテールアプリケーション](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 、または [PowershellのGet-Contentコマンドの使用が必要](https://stackoverflow.com/a/46444596/133936)。

## Dispatcher ログ

ディスパッチャーログは、呼び出し時にstdoutに出力されます `bin/docker_run` が、Dockerに格納されている内のでログに直接アクセスできます。

### Dockerコンテナ内のログへのアクセス

ディスパッチャーログは、のDockerコンテナで直接アクセスでき `/etc/httpd/logs`ます。

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

### Dockerログをローカルファイルシステムにコピーする

ディスパッチャーログは、Dockerコンテナからローカルファイルシステム `/etc/httpd/logs` にコピーし、お気に入りのログ分析ツールを使用して検査できます。 これはポイント・イン・タイム・コピーであり、ログにリアルタイムの更新を提供しません。

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

