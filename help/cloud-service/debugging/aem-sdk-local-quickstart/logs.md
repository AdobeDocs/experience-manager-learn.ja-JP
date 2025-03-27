---
title: ログを使用したAEM SDK のデバッグ
description: ログは AEM アプリケーションのデバッグの最前線として機能しますが、デプロイされた AEM アプリケーションでの適切なログの記録に依存します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# ログを使用したAEM SDK のデバッグ

AEM SDK のログにアクセスすると、AEM SDK ローカルクイックスタート Jar または Dispatcher ツールのいずれかで、AEM アプリケーションのデバッグに関する重要なインサイトを得ることができます。

## AEM ログ

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

ログは AEM アプリケーションのデバッグの最前線として機能しますが、デプロイされた AEM アプリケーションでの適切なログの記録に依存します。 アドビでは、AEM SDK のローカルクイックスタートおよび AEM as a Cloud Service の開発環境でのログの表示を正規化し、設定の煩雑さや再デプロイを減らすため、ローカル開発と AEM as a Cloud Service 開発のログ設定をできるだけ類似したものにすることをお勧めします。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype) は、次の場所にある Sling Logger OSGi 設定を使用して、ローカル開発用の AEM アプリケーションの Java パッケージのデバッグレベルでログを設定します。

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

`error.log` にログを記録します。

デフォルトのログがローカル開発に不十分な場合、AEM SDK のローカルクイックスタートのログサポート web コンソール（[/system/console/slinglog](http://localhost:4502/system/console/slinglog)）でアドホックログを設定できます。ただし、AEM as a Cloud Service の開発環境でも同じログ設定が必要な場合を除き、アドホックな変更を Git に保持することは推奨されません。 ログサポートコンソールを使用した変更は、AEM SDK のローカルクイックスタートのリポジトリに直接保持されることに注意してください。

Java ログステートメントは、`error.log` ファイルで表示できます。

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

多くの場合、`error.log` で、出力を端末にストリーミングする「tail」を使用すると便利です。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows では、[サードパーティの tail アプリケーション](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)または [Powershell の Get-Content コマンド](https://stackoverflow.com/a/46444596/133936)を使用する必要があります。

## Dispatcher ログ

`bin/docker_run` が呼び出されると、Dispatcher のログが stdout に出力されますが、ログは Docker コンテナ内で直接アクセスできます。

### Docker コンテナ内のログへのアクセス{#dispatcher-tools-access-logs}

`/etc/httpd/logs` の Docker コンテナで Dispatcher ログに直接アクセスできます。

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

_`docker exec -it <CONTAINER ID> /bin/sh` の `<CONTAINER ID>` を、`docker ps` コマンドからリストされたターゲット Docker コンテナ ID に置き換える必要があります。_


### Docker ログのローカルファイルシステムへのコピー{#dispatcher-tools-copy-logs}

`/etc/httpd/logs` の Docker コンテナから Dispatcher ログをローカルファイルシステムにコピーして、お気に入りのログ分析ツールを使用して検査できます。これはある時点のコピーで、ログにリアルタイムで更新されるわけではありません。

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

_`docker cp <CONTAINER_ID>:/var/log/apache2 ./` の `<CONTAINER_ID>` を、`docker ps` コマンドからリストされたターゲット Docker コンテナ ID に置き換える必要があります。_
