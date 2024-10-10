---
title: AEM as a Cloud Service 開発用の Dispatcher ツールを設定する
description: AEM SDK の Dispatcher ツールを使用すると、Dispatcher をローカルで容易にインストール、実行およびトラブルシューティングできるようになるので、Adobe Experience Manager（AEM）プロジェクトのローカル開発が簡単になります。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1621'
ht-degree: 100%

---

# ローカル Dispatcher ツールを設定する {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="ローカル Dispatcher ツール"
>abstract="Dispatcher は、Experience Manager アーキテクチャ全体に不可欠な要素であり、ローカル開発セットアップの一部になります。AEM as a Cloud Service SDK には、推奨 Dispatcher ツールのバージョンが含まれているので、Dispatcher をローカルで容易に設定、検証、シミュレーションできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=ja" text="クラウド内 Dispatcher"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=ja" text="AEM as a Cloud Service SDK のダウンロード"

Adobe Experience Manager（AEM）の Dispatcher は、CDN と AEM パブリッシュ層の間のセキュリティとパフォーマンスのレイヤーを提供する Apache HTTP web サーバーモジュールです。Dispatcher は、Experience Manager アーキテクチャ全体に不可欠な要素であり、ローカル開発セットアップの一部になります。

AEM as a Cloud Service SDK には、Dispatcher の設定、検証およびシミュレーションをローカルで容易に行えるようにする推奨 Dispatcher ツールバージョンが含まれています。Dispatcher ツールは、以下の要素で構成されています。

+ Apache HTTP web サーバーと Dispatcher 設定ファイルのベースラインセット（`.../dispatcher-sdk-x.x.x/src` にあります）
+ `.../dispatcher-sdk-x.x.x/bin/validate` に置かれた設定バリデーター CLI ツール
+ `.../dispatcher-sdk-x.x.x/bin/validator` に置かれた設定生成 CLI ツール
+ `.../dispatcher-sdk-x.x.x/bin/docker_run` に置かれた設定デプロイメント CLI ツール
+ CLI ツールを上書きする不変の設定ファイル（`.../dispatcher-sdk-x.x.x/bin/update_maven` にあります）
+ Dispatcher モジュールで Apache HTTP web サーバーを実行する Docker 画像

`~` は、ユーザーのディレクトリの短縮形として使用されることにご注意ください。 Windows の場合、これは `%HOMEPATH%` に相当します。

>[!NOTE]
>
> このページのビデオは macOS で録画されました。Windows ユーザーはこれに従うことができますが、各ビデオで提供されている同等の Dispatcher ツール Windows コマンドを使用します。

## 前提条件

1. Windows ユーザーは、Windows 10 Professional（または Docker をサポートするバージョン）を使用する必要があります。
1. [Experience Manager パブリッシュクイックスタート JAR](./aem-runtime.md) を、ローカルの開発マシン上にインストールします。

+ オプションとして、ローカルの AEM Publish サービスに最新の [AEM リファレンス web サイト](https://github.com/adobe/aem-guides-wknd/releases)をインストールします。この web サイトは、作業中の Dispatcher を視覚化するために、このチュートリアルで使用されます。

1. [Docker](https://www.docker.com/)（Docker Desktop 2.2.0.5 以降および Docker Engine v19.03.9以降）の最新バージョンをローカル開発マシンにインストールして起動します。

## Dispatcher ツールのダウンロード（AEM SDK の一部として）

AEM as a Cloud Service SDK（AEM SDK）には、開発用に Dispatcher モジュールで Apache HTTP web サーバーをローカルに実行するための Dispatcher ツールと、互換性のある QuickStart Jar が含まれています。

AEM as a Cloud Service SDK を既にダウンロードして[ローカル AEM ランタイムをセットアップ](./aem-runtime.md)してある場合は、再度ダウンロードする必要はありません。

1. Adobe ID を使用して、[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) にログインします。
   + AEM as a Cloud Service SDK をダウンロードするには、Adobe 組織が AEM as a Cloud Service 用にプロビジョニングされている&#x200B;__必要があります__
1. 最新の __AEM SDK__ 結果レコードをクリックしてダウンロードします。

## AEM SDK zip から Dispatcher ツールを抽出

>[!TIP]
>
> Windows ユーザーは、ローカル Dispatcher ツールを含むフォルダーへのパスに、スペースや特殊文字を含めることはできません。 パスにスペースが存在する場合、`docker_run.cmd` は失敗します。

Dispatcher ツールのバージョンは、AEM SDK のバージョンとは異なります。Dispatcher ツールのバージョンが、AEM as a Cloud Service バージョンと一致する AEM SDK バージョンから提供されていることを確認します。

1. ダウンロードした `aem-sdk-xxx.zip` ファイルを解凍します。
1. Dispatcher ツールを `~/aem-sdk/dispatcher` に解凍します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

`aem-sdk-dispatcher-tools-x.x.x-windows.zip` を `C:\Users\<My User>\aem-sdk\dispatcher` に解凍します（必要に応じて、欠落しているフォルダーを作成します）。

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

以下で発行されるすべてのコマンドでは、展開する Dispatcher ツール内容が現在の作業ディレクトリに含まれていると仮定しています。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*このビデオでは説明のために macOS を使用しています。Windows／Linux 版の同等のコマンドを使用すれば、同様の結果を得ることができます。*

## Dispatcher 設定ファイルについて

>[!TIP]
>  [AEM Project Maven アーキタイプ](https://github.com/adobe/aem-project-archetype)で作成された Experience Manager プロジェクトは、この一連の Dispatcher 設定ファイルに事前に設定されているので、Dispatcher ツールの src フォルダーからコピーする必要はありません。

Dispatcher ツールは、ローカル開発を含むすべての環境の動作を定義する、Apache HTTP web サーバーと Dispatcher 設定ファイルのセットを提供します。

これらのファイルは、Experience Manager Maven プロジェクトにコピーして `dispatcher/src` フォルダーに保存するためのものです（Experience Manager Maven プロジェクトにまだ存在しない場合）。

設定ファイルに関する完全な説明は、展開された Dispatcher ツールでは `dispatcher-sdk-x.x.x/docs/Config.html` として利用できます。

## 設定を検証

オプションで、Dispatcher および Apache web サーバー設定（`httpd -t`経由）は、 `validate` スクリプト（`validator` 実行可能と混同しないこと）を使用して検証できます。 `validate` スクリプトは、`validator` の [3 つのフェーズ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=ja)の実行に便利です。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Dispatcher のローカルでの実行

AEM Dispatcher は、`src` Dispatcher および Apache web サーバーの設定ファイルに対して Docker を使用してローカルで実行されます 。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload` 実行可能ファイルは、`docker_run` を手動で終了して再起動する必要がなく、設定ファイルを変更するとリロードするので、`docker_run` よりも推奨されます。または、`docker_run` を使用することができますが、設定ファイルを変更した際は、`docker_run` を手動で終了して再起動する必要があります。

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload` 実行可能ファイルは、`docker_run` を手動で終了して再起動する必要がなく、設定ファイルを変更するとリロードするので、`docker_run` よりも推奨されます。または、`docker_run` を使用することができますが、設定ファイルを変更した際は、`docker_run` を手動で終了して再起動する必要があります。

>[!ENDTABS]

`<aem-publish-host>` は `host.docker.internal` に設定できます。これは、Docker がコンテナで指定し、ホストマシンの IP に解決される特別な DNS 名です。`host.docker.internal` が解決されない場合は、以下の[トラブルシューティング](#troubleshooting-host-docker-internal)の節を参照してください。

例えば、Dispatcher ツールが提供するデフォルトの設定ファイルを使用して Dispatcher Docker コンテナを起動するには、次のようにします。

Dispatcher 設定の src フォルダーへのパスを指定する、Dispatcher Docker コンテナを起動します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

AEM as a Cloud Service SDK のパブリッシュサービスは、ポート 4503 でローカルに動作しており、`http://localhost:8080` の Dispatcher から利用できます。

Experience Manager プロジェクトの Dispatcher 設定に対して Dispatcher ツールを実行するには、プロジェクトの `dispatcher/src` フォルダーを指定します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher ツールのログ

Dispatcher ログは、ローカル開発中に HTTP リクエストがブロックされているかどうか、およびその理由を理解するのに役立ちます。ログレベルは、`docker_run` の実行の前に環境パラメーターを付けることで設定することができます。

`docker_run` が実行されていると、Dispatcher ツールのログが標準出力に出力されます。

Dispatcher のデバッグに役立つパラメーターは以下のとおりです。

+ `DISP_LOG_LEVEL=Debug` は、Dispatcher モジュールのログをデバッグ レベルに設定します
   + デフォルト値：`Warn`
+ `REWRITE_LOG_LEVEL=Debug` は、Apache HTTP web サーバー書き換えモジュールのログ記録をデバッグ レベルに設定します
   + デフォルト値：`Warn`
+ `DISP_RUN_MODE` は Dispatcher 環境の「実行モード」を設定し、対応する実行モードの Dispatcher 設定ファイルを読み込みます。
   + デフォルトは `dev`
+ 有効な値： `dev`、`stage` または `prod`

1 つ以上のパラメーターを `docker_run` に渡すことができます。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### ログファイルへのアクセス

Apache web サーバーと AEM Dispatcher のログには、Docker コンテナー内で直接アクセスすることができます。

+ [Docker コンテナ内のログへのアクセス](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Docker ログのローカルファイルシステムへのコピー](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher ツールを更新するタイミング{#dispatcher-tools-version}

Dispatcher ツールのバージョン番号は、Experience Manager よりもインクリメントされる頻度が低いため、ローカル開発環境で Dispatcher ツールのアップデートの頻度が低くなります。

推奨される Dispatcher ツールバージョンは、AEM as a Cloud Service SDK にバンドルされているもので、Experience Manager as a Cloud Service バージョンと一致します。AEM as a Cloud Service のバージョンは、[Cloud Manager](https://my.cloudmanager.adobe.com/) で見つけることができます。

+ __Cloud Manager／Environments__、__AEM リリース__ ラベルで指定された環境ごとに

![Experience Manager のバージョン](./assets/dispatcher-tools/aem-version.png)

*Dispatcher ツールのバージョンは、Experience Manager のバージョンと一致しないことに注意してください。*

## Apache 設定と Dispatcher 設定のベースラインセットを更新する方法

Apache 設定と Dispatcher 設定のベースラインセットは定期的に強化され、AEM as a Cloud Service SDK バージョンと共にリリースされます。ベストプラクティスとしては、強化されたベースライン設定を AEM プロジェクトに取り込み、[ローカル検証](#validate-configurations)や Cloud Manager パイプラインのエラーを回避するとよいでしょう。`.../dispatcher-sdk-x.x.x/bin` フォルダーの `update_maven.sh` スクリプトを使用して更新します。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*このビデオでは説明のために macOS を使用しています。Windows／Linux 版の同等のコマンドを使用すれば、同様の結果を得ることができます。*


[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用して以前に AEM プロジェクトを作成し、ベースラインの Apache 設定と Dispatcher 設定が最新であったと仮定します。これらのベースライン設定を使用して、`dispatcher/src/conf.d` フォルダーと `dispatcher/src/conf.dispatcher.d` フォルダーから `*.vhost`、`*.conf`、`*.farm`、`*.any` などのファイルを再利用およびコピーすることでプロジェクト固有の設定が作成されました。ローカルの Dispatcher 検証と Cloud Manager のパイプラインは正常に動作していました。

一方、ベースラインの Apache 設定と Dispatcher 設定は、新機能、セキュリティ修正、最適化など、様々な理由で強化されました。それらは、AEM as a Cloud Service のリリースの一環として、Dispatcher ツールの新しいバージョンを通じてリリースされます。

これで、プロジェクト固有の Dispatcher 設定を最新バージョンの Dispatcher ツールに対して検証すると、失敗するようになります。これを解決するには、以下の手順に従ってベースライン設定を更新する必要があります。

+ 最新バージョンの Dispatcher ツールに対して検証が失敗するかどうかを確認します。

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ `update_maven.sh` スクリプトを使用して不変ファイルを更新します。

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ `dispatcher_vhost.conf`、`default.vhost`、`default.farm` などの更新された不変ファイルを検証し、必要に応じて、これらのファイルから派生したカスタムファイルに、適切な変更を加えます。

+ 設定を再検証すると、合格するはずです。

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ ローカルで変更を検証したら、更新された設定ファイルをコミットします。

## トラブルシューティング

### docker_run を実行すると、「host.docker.internal が使用可能になるまで待機中」というメッセージが表示されます{#troubleshooting-host-docker-internal}

`host.docker.internal` は Docker コンテナに指定されるホスト名であり、ホストに解決されます。docs.docker.com によれば、次のとおりです（[macOS](https://docs.docker.com/desktop/networking/)、[Windows](https://docs.docker.com/desktop/networking/)）。

> Docker 18.03 以降のレコメンデーションは、特別な DNS 名 host.docker.internal に接続することです。この DNS 名は、ホストが使用する内部 IP アドレスに解決されます。

`bin/docker_run src host.docker.internal:4503 8080` の結果、__Waiting until host.docker.internal is available__（host.docker.internal が使用可能になるまで待機中）というメッセージが表示された場合は、以下を実行します。

1. インストールされている Docker のバージョンが 18.03 以降であることを確認します。
1. セットアップされているローカルマシンにより、`host.docker.internal` の名前の登録／解決が妨げられている可能性があります。代わりに、ローカル IP を使用します。

>[!BEGINTABS]

>[!TAB macOS]

+ ターミナルから `ifconfig` を実行し、Host __inet__ IPアドレス（通常は __en0__ デバイス）を記録します。
+ 次に、ホスト IP アドレスを使って `docker_run` を実行します：`$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ コマンドプロンプトから `ipconfig` を実行し、ホストマシンの __IPv4 アドレス__&#x200B;を記録します。
+ そして、この IP アドレスを使用して `docker_run` を実行します。`$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ ターミナルから `ifconfig` を実行し、Host __inet__ IPアドレス（通常は __en0__ デバイス）を記録します。
+ 次に、ホスト IP アドレスを使って `docker_run` を実行します。`$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### エラー例

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## その他のリソース

+ [AEM SDK をダウンロード](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker をダウンロード](https://www.docker.com/)
+ [AEM 参照用 web サイト（WKND）をダウンロード](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher のドキュメント](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja)
