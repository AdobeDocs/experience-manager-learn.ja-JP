---
title: Dispatcherツールのデバッグ
description: Dispatcherツールは、コンテナ化されたApache Webサーバー環境を提供します。この環境を使用して、AEM as a  as a Cloud ServicesのAEMパブリッシュサービスのDispatcherをローカルでシミュレートできます。 エンドツーエンドのAEMアプリケーションと、サポートするキャッシュとセキュリティの設定が正しいことを確認するには、Dispatcherツールのログとキャッシュのコンテンツのデバッグが不可欠です。
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: 開発
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# Dispatcherツールのデバッグ

Dispatcherツールは、コンテナ化されたApache Webサーバー環境を提供します。この環境を使用して、AEM as a  as a Cloud ServicesのAEMパブリッシュサービスのDispatcherをローカルでシミュレートできます。
エンドツーエンドのAEMアプリケーションと、サポートするキャッシュとセキュリティの設定が正しいことを確認するには、Dispatcherツールのログとキャッシュのコンテンツのデバッグが不可欠です。

>[!NOTE]
>
>Dispatcherツールはコンテナベースなので、再起動するたびに、以前のログとキャッシュの内容が破棄されます。

## Dispatcherツールログ

Dispatcherツールのログは、 `stdout`または`bin/docker_run`コマンドを使用して、または`/etc/https/logs`のDockerコンテナで詳しく確認できます。

DispatcherツールのDockerコンテナのログに直接アクセスする方法については、[Dispatcherのログ](./logs.md#dispatcher-logs)を参照してください。

## Dispatcherツールのキャッシュ

### Dockerコンテナでのログへのアクセス

Dispatcherキャッシュは、` /mnt/var/www/html`にあるDockerコンテナ内で直接アクセスできます。

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

### Dockerログのローカルファイルシステムへのコピー

Dispatcherログは、 `/mnt/var/www/html`にあるDockerコンテナからローカルファイルシステムにコピーし、お気に入りのツールを使用して検査できます。 これはポイントインタイムコピーで、キャッシュに対してリアルタイム更新を提供しません。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

