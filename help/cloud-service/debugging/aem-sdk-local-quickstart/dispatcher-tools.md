---
title: Dispatcher ツールのデバッグ
description: Dispatcher ツールは、コンテナ化された Apache web サーバー環境を提供します。この環境を使用して、AEM as a Cloud Services の AEM パブリッシュサービスの Dispatcher をローカルでシミュレートできます。Dispatcher ツールのログとキャッシュコンテンツのデバッグは、エンドツーエンドの AEM アプリケーションと、サポートするキャッシュおよびセキュリティ設定が正しいことを確認するために不可欠です。
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Dispatcher ツールのデバッグ

Dispatcher ツールは、コンテナ化された Apache web サーバー環境を提供します。この環境を使用して、AEM as a Cloud Services の AEM パブリッシュサービスの Dispatcher をローカルでシミュレートできます。

Dispatcher ツールのログとキャッシュコンテンツのデバッグは、エンドツーエンドの AEM アプリケーションと、サポートするキャッシュおよびセキュリティ設定が正しいことを確認するために不可欠です。

>[!NOTE]
>
>Dispatcher ツールはコンテナベースなので、再起動するたびに、以前のログとキャッシュの内容が破棄されます。

## Dispatcher ツールのログ

Dispatcher ツールのログは、`stdout` または `bin/docker_run` コマンドを介して利用できます。詳細については、`/etc/https/logs` の Docker コンテナで利用できます。

Dispatcher ツールの Docker コンテナのログに直接アクセスする方法については、[Dispatcher のログ](./logs.md#dispatcher-logs)を参照してください。

## Dispatcher ツールのキャッシュ

### Docker コンテナ内のログへのアクセス

Dispatcher のキャッシュは、` /mnt/var/www/html` の Docker コンテナ内で直接アクセスできます。

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

### Docker ログのローカルファイルシステムへのコピー

Dispatcher のログは、お気に入りのツールを使用して検査するために、`/mnt/var/www/html` の Docker コンテナからローカルファイルシステムにコピーできます。これはその時点でのコピーであり、キャッシュに対するリアルタイムのアップデートは提供されません。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
