---
title: ログ
description: ログは、AEM as a Cloud ServiceでAEMアプリケーションをデバッグする際の最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---

# ログを使用したAEMのas a Cloud Service的なデバッグ

ログは、AEM as a Cloud ServiceでAEMアプリケーションをデバッグする際の最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。

特定の環境のAEMサービス（オーサー、パブリッシュ/パブリッシュ Dispatcher）のすべてのログアクティビティは、そのサービス内の別のポッドがログステートメントを生成した場合でも、1 つのログファイルに統合されます。

ポッド ID は各ログステートメントに提供され、ログステートメントのフィルタリングや照合を可能にします。 ポッド ID の形式は次のとおりです。

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 例：`cm-p12345-e56789-aem-author-abcdefabde-98765`

## カスタムログファイル

AEM as aCloud Servicesはカスタムログファイルをサポートしませんが、カスタムログはサポートしています。

AEMas a Cloud Service( [Cloud Manager](#cloud-manager) または [Adobe I/OCLI](#aio))、カスタムログステートメントを書き込む必要があります `error.log`. カスタムの名前付きログに書き込まれたログ（例： ） `example.log`は、AEM as a Cloud Serviceからアクセスできません。

ログは `error.log` アプリケーションの `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` ファイル。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM オーサーとパブリッシュのサービスログ

AEM オーサーサービスとパブリッシュサービスの両方で、AEMランタイムサーバーログを提供します。

+ `aemerror` は Java エラーログです（次の場所にあります）。 `/crx-quickstart/logs/error.log` (AEM SDK ローカルクイックスタート )。 次に、 [推奨ログレベル](#log-levels) 環境タイプごとのカスタムロガーの場合：
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`
+ `aemaccess` AEMサービスへの HTTP リクエストと詳細を表示します
+ `aemrequest` に、AEMサービスに対しておこなわれた HTTP リクエストと、それに対応する HTTP 応答を示します

## AEM Publish Dispatcher ログ

Apache Web サーバーと Dispatcher ログは AEM パブリッシュ層にのみ存在し、AEM オーサー層には存在しないので、提供するのは AEM パブリッシュ Dispatcher のみです。

+ `httpdaccess` に、AEMサービスの Apache Web サーバー/Dispatcher に対しておこなわれた HTTP リクエストを示します。
+ `httperror`  には、Apache Web サーバーからのログメッセージと、 `mod_rewrite`.
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`
+ `aemdispatcher` に、Dispatcher モジュールからのログメッセージを示します。これには、キャッシュメッセージのフィルタリングや処理が含まれます。
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`

## Cloud Manager{#cloud-manager}

AdobeCloud Manager では、環境のログのダウンロードアクションを使用して、日別にログをダウンロードできます。

![Cloud Manager — ログをダウンロード](./assets/logs/download-logs.png)

これらのログは、任意のログ分析ツールを使用してダウンロードし、調査できます。

## Adobe I/OCLI と Cloud Manager プラグイン{#aio}

Adobeの Cloud Manager では、 [Adobe I/OCLI](https://github.com/adobe/aio-cli) と [Adobe I/OCLI 用の Cloud Manager プラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager).

まず、 [Cloud Manager プラグインでのAdobe I/Oの設定](../../local-development-environment/development-tools.md#aio-cli).

関連するプログラム ID と環境 ID が識別されていることを確認し、 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) に使用するログオプションを一覧表示するには [テール](#aio-cli-tail-logs) または [ダウンロード](#aio-cli-download-logs) ログ。

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### ログの追跡{#aio-cli-tail-logs}

Adobe I/OCLI は、AEM as a Cloud Serviceから、 [テールログ](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) コマンドを使用します。 テーリングは、AEMas a Cloud Service環境でアクションが実行されるので、リアルタイムログアクティビティを見るのに役立ちます。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

その他のコマンドラインツール ( `grep` ～と合わせて使える `tail-logs` 対象のログ文を分離するには、次のようにします。

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

は、 `com.example.MySlingModel` またはその文字列を含めます。

### ログのダウンロード{#aio-cli-download-logs}

Adobe I/OCLI は、AEM as a Cloud Serviceからログをダウンロードする機能を提供し、 [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) コマンドを使用します。 これにより、 Cloud Manager Web UI からログをダウンロードするのと同じ結果が得られますが、違いは `download-logs` コマンドは、要求されたログの日数に基づいて、日をまたいでログを統合します。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## ログについて

AEM as a Cloud Serviceのログには、ログステートメントを書き込む複数のポッドがあります。 複数のAEMインスタンスが同じログファイルに書き込むので、デバッグ中に分析方法を理解し、ノイズを減らすことが重要です。 説明するには、次のようにします。 `aemerror` ログスニペットを使用します。

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

日付と時刻の後のデータポイントであるポッド ID を使用して、ログをサービス内のポッドまたはAEMインスタンスで照合し、コードの実行を簡単にトレースして理解できます。

__ポッド cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__ポッド cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 推奨されるログレベル{#log-levels}

AdobeのAEMas a Cloud Service環境ごとのログレベルに関する一般的なガイダンスは次のとおりです。

+ ローカル開発(AEM SDK): `DEBUG`
+ 開発: `DEBUG`
+ ステージ: `WARN`
+ 実稼動: `ERROR`

各環境タイプに対して最も適切なログレベルを設定するには、AEM as a Cloud Serviceを使用します。ログレベルはコード内で維持されます

+ Java ログの設定は、OSGi 設定で維持されます。
+ Dispatcher プロジェクト内の Apache Web サーバーと Dispatcher ログレベル

...したがって、デプロイメントを変更する必要があります。

### Java ログレベルを設定する環境固有の変数

各環境に静的な既知の Java ログレベルを設定する代わりに、AEMをCloud Serviceの [環境固有の変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) ログレベルをパラメータ化し、 [Adobe I/OCLI と Cloud Manager プラグイン](#aio-cli).

これには、環境固有の変数プレースホルダーを使用するようにログ OSGi 設定を更新する必要があります。 [デフォルト値](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) のログレベルは、次のように設定する必要があります。 [Adobeの推奨](#log-levels). 次に例を示します。

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

このアプローチには、以下の点を考慮する必要があります。

+ [使用できる環境変数の数は限られています](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)ログレベルを管理する変数を作成すると、その変数が使用されます。
+ 環境変数は、 [Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) または [Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ 環境変数の変更は、サポートされているツールで手動でリセットする必要があります。 実稼動などの高トラフィック環境をより詳細なログレベルにリセットし忘れると、ログが氾濫し、AEMのパフォーマンスに影響を与える可能性があります。

_環境固有の変数は、Apache Web サーバーや Dispatcher ログの設定に対しては機能しません。OSGi 設定を介して設定されるのでです。_
