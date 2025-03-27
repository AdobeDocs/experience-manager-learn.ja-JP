---
title: ログ
description: ログは、AEM as a Cloud Service で AEM アプリケーションをデバッグする最前線として機能しますが、デプロイされる AEM アプリケーションでの適切なログ記録に依存します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '948'
ht-degree: 100%

---

# ログを使用した AEM as a Cloud Service のデバッグ

ログは、AEM as a Cloud Service で AEM アプリケーションをデバッグする最前線として機能しますが、デプロイされる AEM アプリケーションでの適切なログ記録に依存します。

特定の環境の AEM サービス（オーサー、パブリッシュ／パブリッシュ Dispatcher）のすべてのログアクティビティは、そのサービス内の別のポッドがログステートメントを生成する場合でも、1 つのログファイルに統合されます。

ポッド ID は各ログステートメントに提供され、ログステートメントのフィルタリングや照合を可能にします。ポッド ID の形式は次のとおりです。

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 例：`cm-p12345-e56789-aem-author-abcdefabde-98765`

## カスタムログファイル

AEM as a Cloud Services はカスタムログファイルをサポートしていませんが、カスタムログはサポートしています。

Java ログを AEM as a Cloud Service で（[Cloud Manager](#cloud-manager) または [Adobe I/O CLI](#aio) 経由で）使用できるようにするには、カスタムログステートメントに `error.log` を書き込む必要があります。`example.log` などのカスタム名のログに書き込まれたログは、AEM as a Cloud Service からアクセスできません。

ログは、アプリケーションの `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` ファイルの Sling LogManager OSGi 設定プロパティを使用して、`error.log` に書き込むことができます。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM オーサーとパブリッシュサービスのログ

AEM オーサーサービスとパブリッシュサービスの両方で、AEM ランタイムサーバーログが提供されています。

+ `aemerror` は Java エラーログです（AEM SDK ローカルクイックスタートの `/crx-quickstart/logs/error.log` にあります）。以下は、環境タイプごとのカスタムロガーの[推奨ログレベル](#log-levels)です。
   + 開発：`DEBUG`
   + ステージ：`WARN`
   + 実稼動：`ERROR`
+ `aemaccess` で、AEM サービスへの HTTP リクエストと詳細を表示します
+ `aemrequest` で、AEM サービスに対して行われた HTTP リクエストと、それに対応する HTTP 応答を表示します

## AEM パブリッシュの Dispatcher ログ

Apache web サーバーと Dispatcher ログは AEM パブリッシュ層にのみ存在し、AEM オーサー層には存在しないので、AEM パブリッシュの Dispatcher でのみ提供されます。

+ `httpdaccess` で、AEM サービスの Apache web サーバー／Dispatcher に対して行われた HTTP リクエストを表示します。
+ `httperror` で、Apache web サーバーからのログメッセージを表示し、`mod_rewrite` などのサポートされている Apache モジュールのデバッグに活用できます。
   + 開発：`DEBUG`
   + ステージ：`WARN`
   + 実稼動：`ERROR`
+ `aemdispatcher` で、キャッシュメッセージからのフィルタリングや処理を含む、Dispatcher モジュールからのログメッセージを表示します。
   + 開発：`DEBUG`
   + ステージ：`WARN`
   + 実稼動：`ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager では、環境の「ログをダウンロード」アクションを使用して、日別にログをダウンロードできます。

![Cloud Manager - ログをダウンロード](./assets/logs/download-logs.png)

これらのログは、任意のログ分析ツールを使用してダウンロードし、調査できます。

## Adobe I/O CLI と Cloud Manager プラグイン{#aio}

Adobe Cloud Manager では、[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager) 用の Cloud Manager プラグインを使用して、[Adobe I/O CLI](https://github.com/adobe/aio-cli) 経由で AEM as a Cloud Service ログにアクセスすることができます。

まず、[Cloud Manager プラグインを使用して Adobe I/O を設定します](../../local-development-environment/development-tools.md#aio-cli)。

関連するプログラム ID と環境 ID が識別されていることを確認し、[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) を使用して、ログの[テール](#aio-cli-tail-logs)または[ダウンロード](#aio-cli-download-logs)に使用するログオプションを表示します。

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

### ログのテール{#aio-cli-tail-logs}

Adobe I/O CLI では、[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) コマンドを使用して、AEM as a Cloud Service からリアルタイムでログをテールすることができます。追跡は、AEM as a Cloud Service 環境でアクションが実行されるときに、リアルタイムのログアクティビティを監視するのに役立ちます。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

`grep` などのその他のコマンドラインツールは、`tail-logs` と組み合わせて使用することができ、関心のあるログステートメントの分離に役立ちます。次に例を示します。

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

`com.example.MySlingModel` から生成されたログステートメント、またはその文字列を含むログステートメントのみを表示します。

### ログのダウンロード{#aio-cli-download-logs}

Adobe I/O CLI では、[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days) コマンドを使用して AEM as a Cloud Service からログをダウンロードすることができます。これにより、Cloud Manager web UI からログをダウンロードした場合と同じ結果が得られますが、`download-logs` コマンドは、要求されたログの日数に基づいて、日をまたいでログを統合します。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## ログについて

AEM as a Cloud Service のログには、ログステートメントを書き込む複数のポッドがあります。複数の AEM インスタンスが同じログファイルに書き込むため、分析方法を理解し、デバッグ中のノイズを減らす必要があります。説明のために、次の `aemerror` ログスニペットを使用します。

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

日時の後のデータポイントであるポッド ID を使用して、ログをサービス内のポッドまたは AEM インスタンスで照合し、コードの実行を容易にトレースして理解できます。

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 推奨ログレベル{#log-levels}

AEM as a Cloud Service 環境ごとのログレベルに関するアドビの一般的なガイダンスは次のとおりです。

+ ローカル開発（AEM SDK）：`DEBUG`
+ 開発：`DEBUG`
+ ステージ：`WARN`
+ 実稼動：`ERROR`

環境のタイプごとに最適なログレベルの設定は AEM as a Cloud Service で行われ、ログレベルはコードで維持管理されます。

+ Java ログの設定は、OSGi 設定で維持管理されます。
+ Dispatcher プロジェクト内の Apache web サーバーおよび Dispatcher ログレベル

そのため、変更するにはデプロイメントが必要です。

### Java ログレベルを設定する環境固有の変数

環境ごとに静的な既知の Java ログレベルを設定する代わりに、AEM as Cloud Service の[環境固有の変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#environment-specific-configuration-values)を使用してログレベルをパラメーター化し、[Adobe I/O CLI と Cloud Manager プラグイン](#aio-cli)から値を動的に変更することができます。

これには、環境固有の変数プレースホルダーを使用するようにログ OSGi 設定を更新する必要があります。 [ログレベルのデフォルト値](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#default-values)は、[アドビのレコメンデーション値](#log-levels)に従って設定する必要があります。次に例を示します。

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

このアプローチでは、次のマイナス面を考慮する必要があります。

+ [使用できる環境変数の数は限られており](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#number-of-variables)、ログレベルを管理する変数を作成すると、その変数が使用されます。
+ 環境変数は、プログラムで [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=ja)、[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)、[Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#cloud-manager-api-format-for-setting-properties) を介して管理できます。
+ 環境変数の変更は、サポートされているツールで手動でリセットする必要があります。実稼動などのトラフィックが多い環境を詳細度の低いログレベルにリセットし忘れると、ログが氾濫し、AEM のパフォーマンスに影響を与える可能性があります。

_環境固有の変数は Apache web サーバーや Dispatcher のログ設定には機能しません。これらが OSGi 設定を通して行われないからです。_
