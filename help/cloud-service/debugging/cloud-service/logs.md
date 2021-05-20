---
title: ログ
description: ログは、AEM as aCloud ServiceでのAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '993'
ht-degree: 3%

---


# ログを使用したCloud ServiceとしてのAEMのデバッグ

ログは、AEM as aCloud ServiceでのAEMアプリケーションのデバッグの最前線として機能しますが、デプロイされるAEMアプリケーションでの適切なログの記録に依存します。

特定の環境のAEMサービス（オーサー、パブリッシュ/パブリッシュDispatcher）のすべてのログアクティビティは、そのサービス内の異なるポッドがログステートメントを生成した場合でも、単一のログファイルに統合されます。

ポッドIDは、各ログステートメントに提供され、ログステートメントのフィルタリングや照合を可能にします。 ポッドIDの形式は次のとおりです。

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## カスタムログファイル

AEM as aCloud Servicesは、カスタムログファイルをサポートしませんが、カスタムログをサポートしています。

AEM as aCloud Service([Cloud Manager](#cloud-manager)または[Adobe I/OCLI](#aio)を介してJavaログを使用できるようにするには、カスタムログステートメントを`error.log`に記述する必要があります。 `example.log`などのカスタム名付きログに書き込まれたログは、AEM as a Cloud Serviceからはアクセスできません。

## AEMオーサーとパブリッシュのサービスログ

AEMオーサーサービスとパブリッシュサービスの両方がAEMランタイムサーバーログを提供します。

+ `aemerror` はJavaエラーログです(AEM SDKのローカ `/crx-quickstart/error.log` ルクイックスタートにあります)。環境タイプごとのカスタムロガーに推奨される[ログレベル](#log-levels)を次に示します。
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`
+ `aemaccess` AEMサービスへのHTTP要求のリストと詳細
+ `aemrequest` AEMサービスに対しておこなわれたHTTPリクエストと、対応するHTTP応答の一覧を表示します

## AEMパブリッシュDispatcherログ

これらの側面はAEMパブリッシュ層にのみ存在し、AEMオーサー層には存在しないので、Apache WebサーバーとDispatcherのログはAEMパブリッシュディスパッチャーのみ提供します。

+ `httpdaccess` は、AEMサービスのApache Webサーバー/Dispatcherに対しておこなわれたHTTP要求をリストします。
+ `httperror`  には、Apache Webサーバーからのログメッセージと、などのサポートされているApacheモジュールのデバッグに役立つ情報が記載されて `mod_rewrite`います。
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`
+ `aemdispatcher` に、キャッシュメッセージのフィルタリングや提供を含む、Dispatcherモジュールのログメッセージを示します。
   + 開発: `DEBUG`
   + ステージ: `WARN`
   + 実稼動: `ERROR`

## Cloud Manager{#cloud-manager}

AdobeCloud Managerでは、環境のログのダウンロードアクションを使用して、日別にログをダウンロードできます。

![Cloud Manager — ダウンロードログ](./assets/logs/download-logs.png)

これらのログは、任意のログ分析ツールを使用してダウンロードおよび調査できます。

## Adobe I/OCLIとCloud Managerプラグイン{#aio}

Cloud Managerでは、[Cloud ManagerAdobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)を使用して、[Adobe I/OCLI](https://github.com/adobe/aio-cli)を介したCloud ServiceログとしてAEMにアクセスできます。

まず、[Cloud ManagerAdobe I/O](../../local-development-environment/development-tools.md#aio-cli)を使用してプラグインを設定します。

関連するプログラムIDと環境IDが特定されていることを確認し、[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)を使用して、[tail](#aio-cli-tail-logs)または[download](#aio-cli-download-logs)ログに使用するログオプションをリストします。

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

Adobe I/OCLIは、[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)コマンドを使用して、AEMからリアルタイムでログをCloud Serviceとして追跡する機能を提供します。 テーリングは、AEM as a Cloud Service環境でアクションが実行されるので、リアルタイムログアクティビティを見るのに役立ちます。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

`grep`などの他のコマンドラインツールは、`tail-logs`と組み合わせて使用し、関心のあるログ文を分離できます。次に例を示します。

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

`com.example.MySlingModel`から生成されたログ文や、その文字列を含むログ文のみを表示します。

### ログ{#aio-cli-download-logs}をダウンロードしています

Adobe I/OCLIは、[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))コマンドを使用して、AEMからログをCloud Serviceとしてダウンロードする機能を提供します。 これにより、 Cloud Manager Web UIからログをダウンロードするのと同じ結果が得られます。`download-logs`コマンドは、ログがリクエストされた日数に基づいてログを日単位で統合します。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## ログについて

AEM as aCloud Serviceとしてログインするには、ログステートメントを書き込む複数のポッドがあります。 複数のAEMインスタンスが同じログファイルに書き込むので、分析方法を理解し、デバッグ中にノイズを減らすことが重要です。 説明するために、次の`aemerror`ログスニペットが使用されます。

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

日時の後のデータポイントであるポッドIDを使用して、ログをサービス内のポッドまたはAEMインスタンスで照合し、コードの実行を簡単にトレースして理解できます。

__ポッドcm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__ポッドcm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 推奨ログレベル{#log-levels}

Adobe環境としてのAEMあたりのログレベルに関するCloud Serviceの一般的なガイダンスは次のとおりです。

+ ローカル開発(AEM SDK):`DEBUG`
+ 開発: `DEBUG`
+ ステージ: `WARN`
+ 実稼動: `ERROR`

各環境タイプに最も適切なログレベルを設定するには、AEMをCloud Serviceとして使用します。ログレベルはコードで維持されます

+ Javaログの設定はOSGi設定で維持されます
+ Dispatcherプロジェクト内のApache WebサーバーとDispatcherログレベル

...したがって、デプロイメントを変更する必要があります。

### Javaログレベルを設定する環境固有の変数

各環境に静的な既知のJavaログレベルを設定する代わりに、AEMをCloud Serviceの[環境変数](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)として使用してログレベルをパラメータ化し、[Adobe I/OCLIとCloud Managerプラグイン](#aio-cli)を使用して値を動的に変更できます。

そのためには、環境固有の変数プレースホルダーを使用するようにログOSGi設定を更新する必要があります。 [ログレ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) ベルのデフォルト値は、 [Adobeの推奨事項](#log-levels)に従って設定する必要があります。以下に例を示します。

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

このアプローチには、考慮する必要がある欠点があります。

+ [使用できる環境変数の数は限られています](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)。ログレベルを管理する変数を作成すると、その変数が使用されます。
+ 環境変数は、[Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)または[Cloud Manager HTTP API](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)を介してのみプログラムで管理できます。
+ 環境変数の変更は、サポートされているツールで手動でリセットする必要があります。 実稼動などの高トラフィック環境をより詳細なログレベルにリセットし忘れると、ログがいっぱいになり、AEMのパフォーマンスに影響を与える可能性があります。

_環境固有の変数は、OSGi設定を介して設定されないので、Apache WebサーバーやDispatcherログ設定に対しては機能しません。_