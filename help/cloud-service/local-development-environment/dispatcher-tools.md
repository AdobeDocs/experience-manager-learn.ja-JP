---
title: Cloud Service開発としてのAEM用のディスパッチャーツールの設定
description: AEM SDKのディスパッチャーツールは、Dispatcherをローカルに簡単にインストール、実行、トラブルシューティングできるようにして、Adobe Experience Manager(AEM)プロジェクトのローカル開発を促進します。
sub-product: 基礎
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 1%

---


# ローカルディスパッチャーツールの設定

Adobe Experience Manager(AEM)のディスパッチャーは、CDNとAEM発行層の間にセキュリティとパフォーマンスの層を提供するApache HTTP Webサーバーモジュールです。 ディスパッチャーは、Experience Managerアーキテクチャ全体の不可欠な要素であり、ローカル開発のセットアップに含める必要があります。

Cloud ServiceSDKとしてのAEMには、推奨されるディスパッチャーツールバージョンが含まれており、これにより、ディスパッチャーをローカルで設定、検証、シミュレーションできます。 ディスパッチャーツールは、次のものから構成されます。

+ Apache HTTP Webサーバーとディスパッチャー設定ファイルのベースラインセット( `.../dispatcher-sdk-x.x.x/src`
+ 設定検証CLIツール( `.../dispatcher-sdk-x.x.x/bin/validator`
+ 設定導入CLIツール( `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ ディスパッチャーモジュールと共にApache HTTP Webサーバーを実行するドッカーイメージ

は、ユーザーのディレクトリの略記法 `~` として使用されます。 Windowsでは、これはと同じで `%HOMEPATH%`す。

>[!NOTE]
>
> このページのビデオはmacOSで録画されています。 Windowsユーザーは、各ビデオに対応する同等のディスパッチャーツールのWindowsコマンドを使用できます。

## 前提条件

1. WindowsユーザーはWindows 10 Professionalを使用する必要がある
1. ローカルの開発マシンにPublish QuickStart [](./aem-runtime.md) Experience Managerをインストールします。
   + 必要に応じて、最新の [AEMリファレンスWebサイト](https://github.com/adobe/aem-guides-wknd/releases) をローカルAEM発行サービスにインストールします。 このWebサイトは、作業中のディスパッチャーを視覚化するためにこのチュートリアルで使用します。
1. 最新バージョンの [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)をローカル開発マシンにインストールして開始します。

## ディスパッチャーツールのダウンロード(AEM SDKの一部として)

Cloud ServiceSDKまたはAEM SDKとしてのAEMには、開発用にApache HTTP Webサーバーをローカルで実行するために使用するディスパッチャーツールと、互換性のあるQuickStart Jarが含まれています。

Cloud ServiceSDKとしてのAEMが既にダウンロードされていて、ローカルAEMランタイムを [セットアップする場合](./aem-runtime.md)、再ダウンロードする必要はありません。

1. Log in to [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) with your Adobe ID
   + AEMをCloud ServiceSDKとしてダウンロードするには、Adobe組織 __がAEM用にCloud Serviceとしてプロビジョニングされている必要があります__ 。
1. 「 __AEM」タブにCloud Serviceとして移動します__ 。
1. __発行日順に並べ替え__ ( ____ 降順順)
1. 最新の __AEM SDK__ 結果行をクリックします
1. EULAを確認して同意し、「 __ダウンロード__ 」ボタンをタップします
1. AEM SDKのディスパッチャーツールv2.0.21以降が使用されていることを確認します。

## AEM SDKのzipからディスパッチャーツールを抽出します。

>[!TIP]
>
> Windowsユーザーは、ローカルディスパッチャーツールを含むフォルダーのパスにスペースや特殊文字を含めることはできません。 パスにスペースが存在する場合、パスは失敗 `docker_run.cmd` します。

ディスパッチャーツールのバージョンは、AEM SDKのバージョンとは異なります。 AEMのCloud Serviceバージョンと一致するAEM SDKバージョンを介して、ディスパッチャーツールのバージョンが提供されていることを確認します。

1. Unzip the downloaded `aem-sdk-xxx.zip` file
1. ディスパッチャーツールの展開先 `~/aem-sdk/dispatcher`
   + Windows:に解凍 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` ( `C:\Users\<My User>\aem-sdk\dispatcher` 必要に応じて見つからないフォルダーを作成)
   + macOS/Linux:付属のシェルスクリプトを実行して、ディスパッチャーツール `aem-sdk-dispatcher-tools-x.x.x-unix.sh` を解凍します。
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

以下に示すすべてのコマンドは、現在の作業ディレクトリに拡張ディスパッチャーツールの内容が含まれていることを前提としています。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。 同様の結果を得るために、同等のWindows/Linuxコマンドを使用できます*

## ディスパッチャー設定ファイルについて理解します。

>[!TIP]
> AEM Project Mavenアーキタイプ [( Project Maven Archetype](https://github.com/adobe/aem-project-archetype) )から作成されたExperience Managerプロジェクトは、この一連のディスパッチャー設定ファイルに事前に設定されているので、ディスパッチャーツール(Dispatcher Tools) srcフォルダーからコピーオーバーする必要はありません。

ディスパッチャーツールには、ローカル開発を含むすべての環境の動作を定義するApache HTTP Webサーバーとディスパッチャー設定ファイルのセットが用意されています。

これらのファイルは、Experience ManagerMavenプロジェクトにまだ存在しない場合は、Experience ManagerMavenプロジェクトにコピーされ `dispatcher/src` ます。

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*このビデオでは、例示的な目的でmacOSを使用しています。 同様の結果を得るために、同等のWindows/Linuxコマンドを使用できます*

設定ファイルの詳細な説明は、ディスパッチャーツールでは、として参照でき `dispatcher-sdk-x.x.x/docs/Config.html`ます。

## ディスパッチャーをローカルで実行

ディスパッチャーをローカルで実行するには、その設定に使用するディスパッチャー設定ファイルを、ディスパッチャーツールの `validator` CLIツールを使用して検証する必要があります。

+ 使用方法:
   + Windows：`bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

検証は、次の2つの目的で行われます。

+ Apache HTTP Webサーバーおよびディスパッチャー設定ファイルの正確性を検証します
+ 設定を、DockerコンテナのApache HTTP Webサーバーと互換性のあるファイルセットに変換します。

検証が完了すると、トランスパイルされた設定は、Dockerコンテナでローカルにディスパッチャーを実行します。 バリデーターの __オプションを使用して、最新の設定が検証__ され `-d` 、出力されていることを確認することが重要です。

+ 使用方法:
   + Windows：`bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

は、に設定 `aem-publish-host` でき `host.docker.internal`ます。これは、ホストマシンのIPに解決されるコンテナでDockerが提供する特殊なDNS名です。 問題が解決しな `host.docker.internal` い場合は、以下の [トラブルシューティング](#troubleshooting-host-docker-internal) の節を参照してください。

例えば、ディスパッチャーツールが提供するデフォルトの設定ファイルを使用して、Dispatcher Dockerコンテナを開始するには：

1. 設定が変更され `deployment-folder`るたびに、規則に従って名前付けさ `out` れたを最初から生成します。

   + Windows：`del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （再）展開フォルダーへのパスを提供する開始ディスパッチャードッカーコンテナ:

   + Windows：`bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Cloud ServiceSDKの発行サービスとしてのAEMは、ローカルでポート4503上を実行しているので、Dispatcherを通じて、次の場所で使用でき `http://localhost:8080`ます。

Experience Managerプロジェクトのディスパッチャー設定に対してディスパッチャーツールを実行するには、プロジェクトの `deployment-folder``dispatcher/src` フォルダーを使用してディスパッチャーを生成するだけです。

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

*このビデオでは、例示的な目的でmacOSを使用しています。 同様の結果を得るために、同等のWindows/Linuxコマンドを使用できます*

## ディスパッチャーツールログ

ディスパッチャーログは、ローカルの開発時に、HTTP要求がブロックされているかどうかとその理由を理解するのに役立ちます。 ログレベルは、の実行を環境パラメーターで事前に定義することで設定 `docker_run` できます。

ディスパッチャーツールのログは、が実行されると標準出力に発行 `docker_run` されます。

ディスパッチャーのデバッグに役立つパラメーターを次に示します。

+ `DISP_LOG_LEVEL=Debug` ディスパッチャーモジュールのログをデバッグレベルに設定します
   + Default value is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` Apache HTTP Webサーバー書き換えモジュールログをデバッグレベルに設定します
   + Default value is: `Warn`
+ `DISP_RUN_MODE` ディスパッチャー環境の「実行モード」を設定し、対応する実行モードのディスパッチャー設定ファイルを読み込みます。
   + デフォルト に設定`dev`
+ 有効な値： `dev`、 `stage`または `prod`

1つ以上のパラメーターを `docker_run`

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

*このビデオでは、例示的な目的でmacOSを使用しています。 同様の結果を得るために、同等のWindows/Linuxコマンドを使用できます*

## ディスパッチャーツールを更新するタイミング{#dispatcher-tools-version}

ディスパッチャーツールのバージョンは、Experience Managerよりも増分の頻度が少ないので、ローカル開発環境では、ディスパッチャーツールの更新が必要となる回数が少なくなります。

推奨されるディスパッチャーツールのバージョンは、AEMにCloud ServiceSDKとしてバンドルされていて、Experience ManagerのCloud Serviceバージョンと一致するものです。 Cloud ServiceとしてのAEMのバージョンは、 [Cloud Managerを使用して確認できます](https://my.cloudmanager.adobe.com/)。

+ __Cloud Manager/環境__(AEM Release ____ ラベルで指定された環境ごと)

![Experience Managerバージョン](./assets/dispatcher-tools/aem-version.png)

_ディスパッチャーツールのバージョン自体は、Experience Managerのバージョンとは一致しません。_

## トラブルシューティング

### docker_runは、「host.docker.internalが使用可能になるまで待機中」というメッセージを返します。{#troubleshooting-host-docker-internal}

`host.docker.internal` は、Dockerに提供されるホスト名に、ホストに解決されるホストが含まれています。 docs.docker.comごと([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)、 [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Docker 18.03以降では、ホストが使用する内部IPアドレスに解決される、特殊なDNS名host.docker.internalに接続することを推奨します。

結果が「host.docker.internalが使用可能になるまで `bin/docker_run out host.docker.internal:4503 8080` 待機中 ____」というメッセージになった場合は、次のようになります。

1. インストールされているDockerのバージョンが18.03以降であることを確認します。
2. ローカルマシンをセットアップし、 `host.docker.internal` 名前の登録/解決を妨げている可能性があります。 代わりに、ローカルIPを使用します。
   + Windows：
      + コマンドプロンプトで、ホストマシンの `ipconfig`IPv4アドレス ____ (IPv4)を実行し、記録します。
      + 次に、次のIPアドレス `docker_run` を使用してを実行します。
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + ターミナルから、ホストの `ifconfig` inet __IPアドレス(通常は__ en0 ____ デバイス)を実行し、記録します。
      + 次に、ホストのIPアドレス `docker_run` を使用して実行します。
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

実行中 `docker_run.cmd`に、次のエラーが表示されます： __**展開フォルダが見つかりません：__。 これは、通常、パスにスペースが含まれているためです。 可能であれば、フォルダー内のスペースを削除するか、スペースを含まないパスに `aem-sdk` フォルダーを移動します。

例えば、Windowsのユーザーフォルダーは `<First name> <Last name>`、間にスペースを入れたフォルダーであることがよくあります。 下の例では、フォルダーに、ローカルディスパッチャーツールの `...\My User\...``docker_run` 実行を中断するスペースが含まれています。 スペースがWindowsユーザーフォルダー内にある場合、Windowsが中断されるので、このフォルダーの名前を変更しないでください。その代わりに、ユーザーが完全に変更する権限を持つ新しい場所にフォルダーを移動してください。 `aem-sdk` ユーザーのホームディレクトリに `aem-sdk` フォルダーがあると想定する手順は、新しい場所に調整する必要があります。

#### エラーの例

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_runがWindowsで開始に失敗する{#troubleshooting-windows-compatible}

Windows `docker_run` で実行すると、次のエラーが発生し、ディスパッチャーを起動できなくなる可能性があります。 これは、Windows上のDispatcherに関して報告された問題で、今後のリリースで修正される予定です。

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
