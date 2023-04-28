---
title: AEM as a Cloud Service 開発用の Dispatcher ツールを設定する
description: AEM SDK の Dispatcher ツールは、Dispatcher をローカルで簡単にインストール、実行、トラブルシューティングできるようにすることで、Adobe Experience Manager(AEM) プロジェクトのローカル開発を容易にします。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 63%

---

# ローカル Dispatcher ツールを設定する {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="ローカル Dispatcher ツール"
>abstract="Dispatcher は、Experience Manager アーキテクチャ全体に不可欠な要素であり、ローカル開発設定の一部です。AEM as a Cloud Service SDK には、推奨 Dispatcher ツールのバージョンが含まれており、これにより、Dispatcher をローカルで容易に設定、検証、シミュレーションできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=ja" text="クラウド内の Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html" text="AEM as a Cloud Service SDK のダウンロード"

Adobe Experience Manager(AEM) の Dispatcher は、CDN と AEM パブリッシュ層の間にセキュリティとパフォーマンスの層を提供する Apache HTTP Web サーバーモジュールです。 Dispatcher は、Experience Manager アーキテクチャ全体に不可欠な要素であり、ローカル開発設定の一部です。

AEM as a Cloud Service SDK には、推奨 Dispatcher ツールのバージョンが含まれており、これにより、Dispatcher をローカルで容易に設定、検証、シミュレーションできます。Dispatcher ツールは、以下の要素で構成されています。

+ Apache HTTP Web サーバーと Dispatcher 設定ファイルのベースラインセット（以下の場所にあります） `.../dispatcher-sdk-x.x.x/src`
+ `.../dispatcher-sdk-x.x.x/bin/validate` に置かれた設定バリデーター CLI ツール
+ `.../dispatcher-sdk-x.x.x/bin/validator` に置かれた設定生成 CLI ツール
+ `.../dispatcher-sdk-x.x.x/bin/docker_run` に置かれた設定デプロイメント CLI ツール
+ 不変構成ファイル上書き CLI ツール ( `.../dispatcher-sdk-x.x.x/bin/update_maven`
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

AEMas a Cloud ServiceSDK(AEM SDK) には、開発用に Dispatcher モジュールを使用して Apache HTTP Web サーバーをローカルで実行するための Dispatcher ツールと、互換性のある QuickStart Jar が含まれています。

AEMas a Cloud ServiceSDK が既に [ローカルAEMランタイムの設定](./aem-runtime.md)を再度ダウンロードする必要はありません。

1. Adobe ID を使用して、[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) にログインします。
   + AEM as a Cloud Service SDK をダウンロードするには、Adobe 組織が AEM as a Cloud Service 用にプロビジョニングされている&#x200B;__必要があります__
1. 最新の __AEM SDK__ 結果レコードをクリックしてダウンロードします。

## AEM SDK zip から Dispatcher ツールを抽出

>[!TIP]
>
> Windows ユーザーは、ローカル Dispatcher ツールを含むフォルダーへのパスに、スペースや特殊文字を含めることはできません。 パスにスペースが存在する場合、 `docker_run.cmd` 失敗しました。

Dispatcher ツールのバージョンは、AEM SDK のバージョンとは異なります。 AEM as a Cloud Serviceのバージョンと一致するAEM SDK のバージョンを介して、Dispatcher ツールのバージョンが提供されていることを確認します。

1. ダウンロードした `aem-sdk-xxx.zip` ファイルを解凍します。
1. Dispatcher ツールを `~/aem-sdk/dispatcher` に解凍します。

+ Windows の場合： `aem-sdk-dispatcher-tools-x.x.x-windows.zip` を `C:\Users\<My User>\aem-sdk\dispatcher` に解凍（必要に応じて、欠落しているフォルダーを作成します）
+ macOS Linux®:付属のシェルスクリプトを実行します `aem-sdk-dispatcher-tools-x.x.x-unix.sh` Dispatcher ツールを解凍します。
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

以下に示すすべてのコマンドは、現在の作業ディレクトリに拡張する Dispatcher ツールの内容が含まれていることを前提としています。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*このビデオでは説明のために macOS を使用しています。同様の結果を得るために、Windows／Linux の同等のコマンドを使用できます.*

## Dispatcher 設定ファイルについて

>[!TIP]
>  [AEM Project Maven アーキタイプ](https://github.com/adobe/aem-project-archetype)で作成された Experience Manager プロジェクトは、この一連の Dispatcher 設定ファイルに事前に設定されているので、Dispatcher ツールの src フォルダーからコピーする必要はありません。

Dispatcher ツールは、ローカル開発を含むすべての環境の動作を定義する、Apache HTTP web サーバーと Dispatcher 設定ファイルのセットを提供します。

これらのファイルは、Experience Manager Maven プロジェクトにコピーして `dispatcher/src` フォルダーに保存します（Experience Manager Maven プロジェクトにまだ存在しない場合）。

設定ファイルに関する完全な説明は、展開された Dispatcher ツールでは `dispatcher-sdk-x.x.x/docs/Config.html` として利用できます。

## 設定を検証

オプションで、Dispatcher および Apache web サーバー設定（`httpd -t`経由）は、 `validate` スクリプト（`validator` 実行可能と混同しないこと）を使用して検証できます。 この `validate` スクリプトを使用すると、 [三相](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) の `validator`.

+ 使用方法：
   + Windows：`bin\validate src`
   + macOS Linux®: `./bin/validate.sh ./src`

## Dispatcher のローカルでの実行

AEM Dispatcher は、`src` Dispatcher および Apache web サーバーの設定ファイルに対して Docker を使用してローカルで実行されます 。

+ 使用方法：
   + Windows：`bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`<aem-publish-host>` は、Docker がコンテナで提供し、ホストマシンの IP に解決される特別な DNS 名である `host.docker.internal` に設定できます。 この `host.docker.internal` が解決されない場合は、 [トラブルシューティング](#troubleshooting-host-docker-internal) 」の節を参照してください。

例えば、Dispatcher ツールが提供するデフォルトの設定ファイルを使用して Dispatcher Docker コンテナを起動するには、次のようにします。

Dispatcher 設定の src フォルダーへのパスを指定する、Dispatcher Docker コンテナを起動します。

+ Windows：`bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

ポート 4503 でローカルに実行されるAEMas a Cloud ServiceSDK のパブリッシュサービスは、Dispatcher を通じて次の場所で使用できます。 `http://localhost:8080`.

Experience Manager プロジェクトの Dispatcher 設定に対して Dispatcher ツールを実行するには、プロジェクトの `dispatcher/src` フォルダーを指定します。

+ Windows：

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

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

+ Windows：

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### ログファイルへのアクセス

Apache web サーバーと AEM Dispatcher のログには、Docker コンテナー内で直接アクセスすることができます。

+ [Docker コンテナ内のログへのアクセス](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Docker ログのローカルファイルシステムへのコピー](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher ツールを更新するタイミング{#dispatcher-tools-version}

Dispatcher ツールのバージョン番号は、Experience Manager よりもインクリメントされる頻度が低いため、ローカル開発環境で Dispatcher ツールのアップデートの頻度が低くなります。

推奨される Dispatcher ツールバージョンは、AEM as a Cloud Service SDK にバンドルされているもので、Experience Manager as a Cloud Service バージョンと一致します。AEM as a Cloud Service のバージョンは、[Cloud Manager](https://my.cloudmanager.adobe.com/) で見つけることができます。

+ __Cloud Manager／Environments__、__AEM リリース__ ラベルで指定された環境ごとに

![Experience Manager のバージョン](./assets/dispatcher-tools/aem-version.png)

*Dispatcher ツールのバージョンは、バージョンのバージョンとは一致しないことに注意してください。Experience Managerのバージョンは一致しません。*

## Apache および Dispatcher 設定のベースラインセットを更新する方法

Apache および Dispatcher 設定のベースラインセットは、AEMas a Cloud ServiceSDK バージョンで定期的に拡張され、リリースされています。 ベストプラクティスは、ベースライン設定の機能強化をAEMプロジェクトに組み込み、 [ローカル検証](#validate-configurations) と Cloud Manager のパイプラインエラー。 を使用して更新します。 `update_maven.sh` スクリプト `.../dispatcher-sdk-x.x.x/bin` フォルダー。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*このビデオでは説明のために macOS を使用しています。同様の結果を得るために、Windows／Linux の同等のコマンドを使用できます.*


過去に、 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)を使用する場合、基本となる Apache および Dispatcher の設定が最新です。 これらのベースライン設定を使用すると、のようなファイルを再利用およびコピーすることで、プロジェクト固有の設定が作成されます。 `*.vhost`, `*.conf`, `*.farm` および `*.any` から `dispatcher/src/conf.d` および `dispatcher/src/conf.dispatcher.d` フォルダー。 ローカルの Dispatcher 検証と Cloud Manager のパイプラインは正常に動作していました。

一方、新機能、セキュリティ修正、最適化など、様々な理由で、ベースラインの Apache および Dispatcher 設定が強化されました。 AEM as a Cloud Serviceリリースの一環として、新しいバージョンの Dispatcher ツールを使用してリリースされます。

これで、プロジェクト固有の Dispatcher 設定を最新の Dispatcher ツールバージョンに対して検証する際に、失敗が開始します。 これを解決するには、次の手順に従ってベースライン設定を更新する必要があります。

+ 最新の Dispatcher ツールバージョンに対する検証が失敗することを確認する

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ を使用した不変ファイルの更新 `update_maven.sh` スクリプト

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

+ 更新された不変ファイルの検証： `dispatcher_vhost.conf`, `default.vhost`、および `default.farm` 必要に応じて、カスタムファイルから派生した関連する変更をカスタムファイルに加えます。

+ 設定を再検証します。

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

+ 変更のローカル検証が完了したら、更新された設定ファイルをコミットします

## トラブルシューティング

### docker_run を実行すると、「host.docker.internal が使用可能になるまで待機中」というメッセージが表示されます{#troubleshooting-host-docker-internal}

この `host.docker.internal` は、ホストに解決されるを含む、Docker に提供されるホスト名です。 docs.docker.com（[macOS](https://docs.docker.com/desktop/networking/)、[Windows](https://docs.docker.com/desktop/networking/)）には、以下の様にあります。

> Docker 18.03 以降では、ホストが使用する内部 IP アドレスに解決される特別な DNS 名 host.docker.internal に接続することをお勧めします

条件 `bin/docker_run src host.docker.internal:4503 8080` 結果はメッセージに含まれます __host.docker.internal が使用可能になるまで待機します__、次に、

1. インストールされている Docker のバージョンが 18.03 以降であることを確認します。
2. ローカルマシンが設定されていて、 `host.docker.internal` 名前。 代わりに、ローカル IP を使用します。
   + Windows：
   + コマンドプロンプトから `ipconfig` を実行し、ホストマシンの __IPv4 アドレス__&#x200B;を記録します。
   + そして、この IP アドレスを使用して `docker_run` を実行します。
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®:
   + ターミナルから `ifconfig` を実行し、Host __inet__ IPアドレス（通常は __en0__ デバイス）を記録します。
   + 次に、ホスト IP アドレスを使って `docker_run` を実行します。
      `bin/docker_run.sh src <HOST IP>:4503 8080`

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
