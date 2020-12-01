---
title: ディスパッチャーツールのデバッグ
description: ディスパッチャーツールは、コンテナ化されたApache Webサーバー環境を提供します。このサーバーを使用して、AEMをCloud ServicesのAEM発行サービスのディスパッチャーとしてローカルにシミュレートできます。 ディスパッチャーツールのログとキャッシュの内容のデバッグは、エンドツーエンドのAEMアプリケーションを確実に作成し、キャッシュとセキュリティの設定をサポートする上で重要です。
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# ディスパッチャーツールのデバッグ

ディスパッチャーツールは、コンテナ化されたApache Webサーバー環境を提供します。このサーバーを使用して、AEMをCloud ServicesのAEM発行サービスのディスパッチャーとしてローカルにシミュレートできます。
ディスパッチャーツールのログとキャッシュの内容のデバッグは、エンドツーエンドのAEMアプリケーションを確実に作成し、キャッシュとセキュリティの設定をサポートする上で重要です。

>[!NOTE]
>
>ディスパッチャーツールはコンテナベースなので、再起動するたびに、以前のログとキャッシュの内容が破棄されます。

## ディスパッチャーツールログ

ディスパッチャーツールのログは、`stdout`または`bin/docker_run`コマンドを通じて、あるいは`/etc/https/logs`のDockerコンテナで入手できます。

ディスパッチャーツールのDockerコンテナのログに直接アクセスする方法については、[ディスパッチャーログ](./logs.md#dispatcher-logs)を参照してください。

## ディスパッチャーツールのキャッシュ

### Dockerコンテナ内のログへのアクセス

ディスパッチャーキャッシュは、` /mnt/var/www/html`のDockerコンテナで直接アクセスできます。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Dockerログをローカルファイルシステムにコピーする

ディスパッチャーログは、`/mnt/var/www/html`のDockerコンテナからローカルファイルシステムにコピーし、お気に入りのツールを使用して検査できます。 これはポイント・イン・タイム・コピーであり、キャッシュに対するリアルタイムの更新は提供されません。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

