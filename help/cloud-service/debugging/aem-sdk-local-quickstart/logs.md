---
title: ログを使用したAEM SDKのデバッグ
description: ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: 開発
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 3%

---


# ログを使用したAEM SDKのデバッグ

AEM SDKのログにアクセスすると、AEM SDKのローカルクイックスタートJARまたはDispatcherツールのいずれかで、AEMアプリケーションのデバッグに関する重要なインサイトを得ることができます。

## AEMログ

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

ログはAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。 Adobeでは、AEM SDKのローカルクイックスタートとAEM as a Cloud ServiceCloud Serviceの開発環境でのログの表示性が正常化され、設定の煩雑さや再デプロイメントが軽減されるので、ローカル開発とAEMの開発ログ設定を可能な限り同じにすることをお勧めします。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)は、にあるSling Logger OSGi設定を使用して、ローカル開発用のAEMアプリケーションのJavaパッケージのデバッグレベルでログを設定します。

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

ログは`error.log`に記録されます。

デフォルトのログがローカル開発に不十分な場合、アドホックログは、AEM SDKのローカルクイックスタートのログサポートWebコンソール([/system/console/slinglog](http://localhost:4502/system/console/slinglog))で設定できますが、Cloud Service開発環境と同じログ設定がAEMで必要な場合を除き、Gitに対するアドホックな変更は推奨されません。 ログサポートコンソールを使用した変更は、AEM SDKのローカルクイックスタートのリポジトリに直接保持されます。

Javaログステートメントは、`error.log`ファイルで表示できます。

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

多くの場合、`error.log`を&quot;tail&quot;にして、端末に出力をストリーミングすると便利です。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windowsには、[サードパーティのテールアプリケーション](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)または[PowershellのGet-Contentコマンド](https://stackoverflow.com/a/46444596/133936)を使用する必要があります。

## Dispatcher ログ

`bin/docker_run`が呼び出されると、Dispatcherログがstdoutに出力されますが、Dockerに格納されているを使用してログに直接アクセスできます。

### Dockerコンテナ{#dispatcher-tools-access-logs}のログへのアクセス

Dispatcherログは、`/etc/httpd/logs`にあるDockerコンテナから直接アクセスできます。

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

_のを、コ `<CONTAINER ID>` マ `docker exec -it <CONTAINER ID> /bin/sh` ンドにリストされたターゲットDocker CONTAINER IDに置き換える必要があ `docker ps` ります。_


### Dockerログをローカルファイルシステムにコピーする{#dispatcher-tools-copy-logs}

Dispatcherログは、 `/etc/httpd/logs`にあるDockerコンテナからローカルファイルシステムにコピーし、お気に入りのログ分析ツールを使用して検査できます。 これはポイントインタイムコピーで、ログにリアルタイムで更新されるわけではありません。

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

_のを、コ `<CONTAINER_ID>` マ `docker cp <CONTAINER_ID>:/var/log/apache2 ./` ンドにリストされたターゲットDocker CONTAINER IDに置き換える必要があ `docker ps` ります。_
