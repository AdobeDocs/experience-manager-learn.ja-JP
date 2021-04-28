---
title: Cloud Service開発としてのAEM用のディスパッチャーツールの設定
description: AEM SDKのディスパッチャーツールは、Dispatcherをローカルに簡単にインストール、実行、トラブルシューティングできるようにして、Adobe Experience Manager(AEM)プロジェクトのローカル開発を促進します。
sub-product: 基礎
feature: ディスパッチャー、開発者ツール
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1639'
ht-degree: 2%

---


# ローカルディスパッチャーツールの設定

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="ローカルディスパッチャーツール"
>abstract="ディスパッチャーは、Experience Managerアーキテクチャ全体の不可欠な要素であり、ローカル開発の設定の一部にする必要があります。 Cloud ServiceSDKとしてのAEMには、推奨されるディスパッチャーツールバージョンが含まれており、これにより、ディスパッチャーをローカルで設定、検証、シミュレーションできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="クラウド内の Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Cloud ServiceSDKとしてAEMをダウンロードする"

Adobe Experience Manager(AEM)のディスパッチャーは、CDNとAEM発行層の間にセキュリティとパフォーマンスの層を提供するApache HTTP Webサーバーモジュールです。 ディスパッチャーは、Experience Managerアーキテクチャ全体の不可欠な要素であり、ローカル開発の設定の一部にする必要があります。

Cloud ServiceSDKとしてのAEMには、推奨されるディスパッチャーツールバージョンが含まれており、これにより、ディスパッチャーをローカルで設定、検証、シミュレーションできます。 ディスパッチャーツールは、次のものから構成されます。

+ `.../dispatcher-sdk-x.x.x/src`にある、Apache HTTP Webサーバーとディスパッチャーの設定ファイルのベースラインセット
+ `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)にある設定検証ツール
+ `.../dispatcher-sdk-x.x.x/bin/validator`にある構成生成CLIツール
+ `.../dispatcher-sdk-x.x.x/bin/docker_run`にある構成導入CLIツール
+ ディスパッチャーモジュールと共にApache HTTP Webサーバーを実行するドッカーイメージ

`~`は、ユーザーのディレクトリの略記法として使用されます。 Windowsでは、`%HOMEPATH%`と同じです。

>[!NOTE]
>
> このページのビデオはmacOSで録画されています。 Windowsユーザーは、各ビデオに対応する同等のディスパッチャーツールのWindowsコマンドを使用できます。

## 前提条件

1. WindowsユーザーはWindows 10 Professionalを使用する必要がある
1. ローカルの開発マシンに[Experience Manager発行クイックスタートJar](./aem-runtime.md)をインストールします。
   + 必要に応じて、最新の[AEMリファレンスWebサイト](https://github.com/adobe/aem-guides-wknd/releases)をローカルAEM発行サービスにインストールします。 このWebサイトは、作業中のディスパッチャーを視覚化するためにこのチュートリアルで使用します。
1. ローカル開発マシンに[Docker](https://www.docker.com/)の最新バージョン(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)をインストールして開始します。

## ディスパッチャーツールのダウンロード(AEM SDKの一部として)

Cloud ServiceSDKまたはAEM SDKとしてのAEMには、開発用にApache HTTP Webサーバーをローカルで実行するために使用するディスパッチャーツールと、互換性のあるQuickStart Jarが含まれています。

Cloud ServiceSDKとしてのAEMが既に[ローカルAEMランタイム](./aem-runtime.md)をセットアップするには、を再ダウンロードする必要はありません。

1. [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=%2Fjcr%3Content%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastModified&amp;by.sort=&amp;layout=リスト&amp;p.offset=0&amp;p.limit=1)にAdobe IDとログインします。
   + Adobe組織&#x200B;__は、AEMをCloud ServiceSDKとしてダウンロードするCloud ServiceとしてAEM用にプロビジョニング__&#x200B;する必要があります
1. 最新の&#x200B;__AEM SDK__&#x200B;結果行をクリックしてダウンロードします
   + AEM SDKのディスパッチャーツールv2.0.29以降がダウンロードの説明に記載されていることを確認します。

## AEM SDKのzipからディスパッチャーツールを抽出します。

>[!TIP]
>
> Windowsユーザーは、ローカルディスパッチャーツールを含むフォルダーのパスにスペースや特殊文字を含めることはできません。 パスにスペースが存在する場合、`docker_run.cmd`は失敗します。

ディスパッチャーツールのバージョンは、AEM SDKのバージョンとは異なります。 AEMのCloud Serviceバージョンと一致するAEM SDKバージョンを介して、ディスパッチャーツールのバージョンが提供されていることを確認します。

1. ダウンロードした`aem-sdk-xxx.zip`ファイルを解凍します。
1. ディスパッチャーツールを`~/aem-sdk/dispatcher`に展開します
   + Windows:`aem-sdk-dispatcher-tools-x.x.x-windows.zip`を`C:\Users\<My User>\aem-sdk\dispatcher`に解凍（必要に応じて見つからないフォルダーを作成）
   + macOS/Linux:付属のシェルスクリプト`aem-sdk-dispatcher-tools-x.x.x-unix.sh`を実行し、ディスパッチャーツールを解凍します。
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

以下に示すすべてのコマンドは、現在の作業ディレクトリに拡張ディスパッチャーツールの内容が含まれていることを前提としています。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。同等のWindows/Linuxコマンドを使って*&#x200B;同様の結果を得ることができます。

## ディスパッチャー設定ファイルについて理解します。

>[!TIP]
> [AEM Project Mavenアーキタイプ](https://github.com/adobe/aem-project-archetype)から作成されたExperience Managerプロジェクトは、この一連のディスパッチャー設定ファイルに事前に設定されているので、ディスパッチャーツールのsrcフォルダーからコピーする必要はありません。

ディスパッチャーツールには、ローカル開発を含むすべての環境の動作を定義するApache HTTP Webサーバーとディスパッチャー設定ファイルのセットが用意されています。

これらのファイルは、Experience ManagerMavenプロジェクトにまだ存在しない場合は、Experience ManagerMavenプロジェクトにコピーされます。`dispatcher/src`

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。同等のWindows/Linuxコマンドを使って*&#x200B;同様の結果を得ることができます。

設定ファイルの詳細な説明は、展開されたディスパッチャーツールでは`dispatcher-sdk-x.x.x/docs/Config.html`として入手できます。

## 設定の検証

オプションで、ディスパッチャーとApache Webサーバーの設定（`httpd -t`経由）は、`validate`スクリプトを使用して検証できます（`validator`実行ファイルと混同しないでください）。

+ 使用方法:
   + Windows：`bin\validate src`
   + macOS/Linux:`./bin/validate.sh ./src`

## ディスパッチャーをローカルで実行

ディスパッチャーをローカルで実行するには、ディスパッチャーツールの`validator` CLIツールを使用してディスパッチャー設定ファイルを生成する必要があります。

+ 使用方法:
   + Windows：`bin\validator full -d out src`
   + macOS/Linux:`./bin/validator full -d ./out ./src`

このコマンドは、設定をDockerコンテナのApache HTTP Webサーバーと互換性のあるファイルセットに変換します。

生成されると、トランスパイルされた設定は、Dockerコンテナでローカルにディスパッチャーを実行するために使用されます。 バリデーターの`-d`オプションを使用して、`validate` __および__&#x200B;出力を使用して最新の設定を検証することが重要です。

+ 使用方法:
   + Windows：`bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux:`./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host`は`host.docker.internal`に設定できます。これは、ホストマシンのIPに解決されるコンテナでDockerが提供する特殊なDNS名です。 `host.docker.internal`が解決しない場合は、下の[トラブルシューティング](#troubleshooting-host-docker-internal)の節を参照してください。

例えば、ディスパッチャーツールが提供するデフォルトの設定ファイルを使用して、Dispatcher Dockerコンテナを開始するには：

1. `deployment-folder`という名前の`out`を、設定が変更されるたびに、規則に従って最初から生成します。

   + Windows：`del /Q out && bin\validator full -d out src`
   + macOS/Linux:`rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （再）展開フォルダーへのパスを提供する開始ディスパッチャードッカーコンテナ:

   + Windows：`bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux:`./bin/docker_run.sh ./out host.docker.internal:4503 8080`

ポート4503でローカルに実行されているCloud ServiceSDKの発行サービスとしてのAEMは、`http://localhost:8080`のディスパッチャーを通じて使用できます。

Experience Managerプロジェクトのディスパッチャー設定に対してディスパッチャーツールを実行するには、プロジェクトの`dispatcher/src`フォルダーを使用して`deployment-folder`を生成します。

+ Windows：

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。同等のWindows/Linuxコマンドを使って*&#x200B;同様の結果を得ることができます。

## ディスパッチャーツールログ

ローカル開発中は、ディスパッチャーログが役立ち、HTTP要求がブロックされているかどうかとその理由を理解できます。 ログレベルは、`docker_run`の実行に環境パラメーターを付加することで設定できます。

ディスパッチャーツールログは、`docker_run`が実行されると標準出力に発行されます。

ディスパッチャーのデバッグに役立つパラメーターを次に示します。

+ `DISP_LOG_LEVEL=Debug` ディスパッチャーモジュールのログをデバッグレベルに設定します
   + デフォルト値：`Warn`
+ `REWRITE_LOG_LEVEL=Debug` Apache HTTP Webサーバー書き換えモジュールログをデバッグレベルに設定します
   + デフォルト値：`Warn`
+ `DISP_RUN_MODE` ディスパッチャー環境の「実行モード」を設定し、対応する実行モードのディスパッチャー設定ファイルを読み込みます。
   + デフォルトは `dev`
+ 有効な値：`dev`、`stage`、または`prod`

`docker_run`に渡すことができる1つ以上のパラメータ

+ Windows：

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。同等のWindows/Linuxコマンドを使って*&#x200B;同様の結果を得ることができます。

### ログファイルアクセス

Apache WebサーバーおよびAEM Dispatcherのログは、Dockerコンテナから直接アクセスできます。

+ [Dockerコンテナ内のログへのアクセス](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Dockerログをローカルファイルシステムにコピーする](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## ディスパッチャーツールを更新するタイミング{#dispatcher-tools-version}

ディスパッチャーツールのバージョンは、Experience Managerよりも増分の頻度が少ないので、ローカル開発環境では、ディスパッチャーツールの更新が必要となる回数は少なくなります。

推奨されるディスパッチャーツールのバージョンは、AEMにCloud ServiceSDKとしてバンドルされていて、Experience ManagerのCloud Serviceバージョンと一致するものです。 Cloud ServiceとしてのAEMのバージョンは、[Cloud Manager](https://my.cloudmanager.adobe.com/)で確認できます。

+ __Cloud Manager/環境__(AEM  ____ Releaselabelで指定された環境ごと)

![Experience Managerバージョン](./assets/dispatcher-tools/aem-version.png)

_ディスパッチャーツールのバージョン自体は、Experience Managerのバージョンとは一致しません。_

## トラブルシューティング

### docker_runは、&#39;host.docker.internalが利用可能になるまで待機中&#39;というメッセージ{#troubleshooting-host-docker-internal}になります。

`host.docker.internal` は、Dockerに提供されるホスト名に、ホストに解決されるホストが含まれています。docs.docker.comごと([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)、[Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Docker 18.03以降では、ホストが使用する内部IPアドレスに解決される、特殊なDNS名host.docker.internalに接続することを推奨します。

`bin/docker_run out host.docker.internal:4503 8080`の結果&#x200B;__Waiting until host.docker.internal is available__&#x200B;というメッセージが表示された場合、次のようになります。

1. インストールされているDockerのバージョンが18.03以降であることを確認します。
2. `host.docker.internal`名前の登録/解決を妨げているローカルマシンを設定している可能性があります。 代わりに、ローカルIPを使用します。
   + Windows：
      + コマンドプロンプトで`ipconfig`を実行し、ホストマシンの&#x200B;__IPv4アドレス__&#x200B;を記録します。
      + 次に、次のIPアドレスを使用して`docker_run`を実行します。
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + ターミナルから、`ifconfig`を実行し、ホスト&#x200B;__inet__&#x200B;のIPアドレス（通常は&#x200B;__en0__&#x200B;デバイス）を記録します。
      + 次に、ホストのIPアドレスを使用して`docker_run`を実行します。
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### エラーの例

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_runは&#39;**エラーになります。展開フォルダが見つかりません&#39;

`docker_run.cmd`を実行すると、__**エラーが表示されます。展開フォルダーが見つかりません：__。 これは、通常、パスにスペースが含まれているためです。 可能であれば、フォルダー内のスペースを削除するか、`aem-sdk`フォルダーをスペースを含まないパスに移動します。

例えば、Windowsユーザーフォルダーの多くは`<First name> <Last name>`で、間にスペースが入ります。 下の例のフォルダー`...\My User\...`には、ローカルディスパッチャーツールの`docker_run`の実行を中断するスペースが含まれています。 スペースがWindowsユーザーフォルダー内にある場合、Windowsが中断されるので、このフォルダーの名前を変更しないでください。代わりに、`aem-sdk`フォルダーを、ユーザーが完全に変更する権限を持つ新しい場所に移動します。 `aem-sdk`フォルダーがユーザーのホームディレクトリにあると想定する手順は、新しい場所に調整する必要があります。

#### エラーの例

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_runがWindowsで開始に失敗する{#troubleshooting-windows-compatible}

Windowsで`docker_run`を実行すると、次のエラーが発生し、ディスパッチャーを起動できなくなる可能性があります。 これは、Windows上のDispatcherに関して報告された問題で、今後のリリースで修正される予定です。

#### エラーの例

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## その他のリソース

+ [AEM SDKのダウンロード](https://experience.adobe.com/#/downloads)
+ [Adobeクラウドマネージャー](https://my.cloudmanager.adobe.com/)
+ [Dockerのダウンロード](https://www.docker.com/)
+ [AEMリファレンスWebサイト(WKND)のダウンロード](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Managerディスパッチャードキュメント](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/dispatcher.html)
