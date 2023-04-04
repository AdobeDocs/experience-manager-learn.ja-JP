---
title: ログを使用したAEM SDK のデバッグ
description: ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# ログを使用したAEM SDK のデバッグ

AEM SDK のログへのアクセスでは、AEM SDK ローカルクイックスタート JAR または Dispatcher ツールのいずれかで、AEMアプリケーションのデバッグに関する重要なインサイトを提供できます。

## AEMログ

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。 Adobeでは、ローカル開発 SDK のローカルクイックスタートとAEM as a Cloud Serviceの開発環境でのログの表示を正常化し、設定の煩雑さや再デプロイを減らすので、AEMとAEMのas a Cloud Service開発ログの設定を可能な限り似たものにすることをお勧めします。

この [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 次の場所にある Sling Logger OSGi 設定を使用して、ローカル開発用のAEMアプリケーションの Java パッケージのデバッグレベルでログを設定します。

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

ログは `error.log`.

デフォルトのログがローカル開発に不十分な場合、アドホックログは、AEM SDK のローカルクイックスタートのログサポート Web コンソール ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)) ですが、AEMas a Cloud Serviceの開発環境でも同じログ設定が必要な場合を除き、アドホックな変更を Git に保持することは推奨されません。 ログサポートコンソールを使用した変更は、AEM SDK のローカルクイックスタートのリポジトリに直接保持されます。

Java ログステートメントは、 `error.log` ファイル：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

多くの場合、 `error.log` これは出力を端末に送る

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows にはが必要です [サードパーティのテールアプリケーション](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) または [Powershell の Get-Content コマンド](https://stackoverflow.com/a/46444596/133936).

## Dispatcher ログ

Dispatcher ログは、 `bin/docker_run` が呼び出されますが、Docker に含まれるでログに直接アクセスできます。

### Docker コンテナ内のログへのアクセス{#dispatcher-tools-access-logs}

Dispatcher ログは、Docker コンテナ ( ) 内のに直接アクセスできます。 `/etc/httpd/logs`.

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

_この `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` は、 `docker ps` コマンドを使用します。_


### Docker ログのローカルファイルシステムへのコピー{#dispatcher-tools-copy-logs}

Dispatcher ログは、Docker コンテナ ( ) からコピーできます。 `/etc/httpd/logs` お気に入りのログ分析ツールを使用して、ローカルファイルシステムを検査用に追加します。 これはポイントインタイムコピーで、ログにリアルタイムで更新されるわけではありません。

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

_この `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` は、 `docker ps` コマンドを使用します。_
