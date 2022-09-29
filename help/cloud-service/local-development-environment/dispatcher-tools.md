---
title: AEMas a Cloud Service開発用の Dispatcher ツールの設定
description: AEM SDK の Dispatcher ツールは、Dispatcher をローカルで簡単にインストール、実行、トラブルシューティングできるようにすることで、Adobe Experience Manager(AEM) プロジェクトのローカル開発を容易にします。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 3%

---

# ローカル Dispatcher ツールの設定 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="ローカル Dispatcher ツール"
>abstract="Dispatcher は、Experience Managerアーキテクチャ全体の不可欠な要素であり、ローカル開発設定の一部にする必要があります。 AEMas a Cloud ServiceSDK には、推奨される Dispatcher ツールバージョンが含まれており、Dispatcher をローカルで設定、検証、シミュレーションできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html?lang=ja" text="クラウド内の Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEMas a Cloud ServiceSDK のダウンロード"

Adobe Experience Manager(AEM) の Dispatcher は、CDN と AEM パブリッシュ層の間にセキュリティとパフォーマンスの層を提供する Apache HTTP Web サーバーモジュールです。 Dispatcher は、Experience Managerアーキテクチャ全体の不可欠な要素であり、ローカル開発設定の一部にする必要があります。

AEMas a Cloud ServiceSDK には、推奨される Dispatcher ツールバージョンが含まれており、Dispatcher をローカルで設定、検証、シミュレーションできます。 Dispatcher ツールは、次の要素で構成されます。

+ Apache HTTP Web サーバーと Dispatcher 設定ファイルのベースラインセット。以下の場所にあります。 `.../dispatcher-sdk-x.x.x/src`
+ 次の場所にある設定バリデーター CLI ツール `.../dispatcher-sdk-x.x.x/bin/validate`
+ 次の場所にある設定生成 CLI ツール `.../dispatcher-sdk-x.x.x/bin/validator`
+ 次の場所にある構成導入 CLI ツール `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ Dispatcher モジュールで Apache HTTP Web サーバーを実行する Docker イメージ

注意： `~` は、ユーザーのディレクトリの略記法として使用されます。 Windows の場合、これは `%HOMEPATH%`.

>[!NOTE]
>
> このページのビデオはmacOSで録画されました。 Windows ユーザーは従うことができますが、各ビデオに付属する、同等の Dispatcher ツール Windows コマンドを使用します。

## 前提条件

1. Windows ユーザーは、Windows 10 Professional （または Docker をサポートするバージョン）を使用する必要があります
1. インストール [Experience Manager公開クイックスタート JAR](./aem-runtime.md) ローカルの開発マシン上で
   + （オプション）最新のをインストールする [AEMリファレンス Web サイト](https://github.com/adobe/aem-guides-wknd/releases) ローカルの AEM パブリッシュサービスで、 この Web サイトは、作業中の Dispatcher を視覚化するために、このチュートリアルで使用されます。
1. 最新バージョンのをインストールして起動する [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5 以降/Docker Engine v19.03.9以降 ) をローカル開発マシンに搭載している。

## Dispatcher ツールのダウンロード (AEM SDK の一部として )

AEMas a Cloud ServiceSDK(AEM SDK) には、開発用に Dispatcher モジュールで Apache HTTP Web サーバーをローカルで実行するための Dispatcher ツールと、互換性のある QuickStart Jar が含まれています。

AEMas a Cloud ServiceSDK が既に [ローカルAEMランタイムの設定](./aem-runtime.md)を再度ダウンロードする必要はありません。

1. にログインします。 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=1) Adobe ID
   + Adobe組織 __必須__ AEM as a Cloud Service SDK をダウンロードするためにAEM as a Cloud Service用にプロビジョニングされている
1. 最新の __AEM SDK__ ダウンロードする結果行

## AEM SDK zip から Dispatcher ツールを抽出します。

>[!TIP]
>
> Windows ユーザーは、ローカル Dispatcher ツールを含むフォルダーへのパスにスペースや特殊文字を含めることはできません。 パスにスペースが存在する場合、 `docker_run.cmd` 失敗します。

Dispatcher ツールのバージョンは、AEM SDK のバージョンとは異なります。 AEM as a Cloud Serviceのバージョンと一致するAEM SDK バージョンを介して、Dispatcher ツールのバージョンが提供されていることを確認します。

1. ダウンロードしたを解凍します。 `aem-sdk-xxx.zip` ファイル
1. Dispatcher ツールを解凍します。 `~/aem-sdk/dispatcher`
   + Windows の場合：解凍 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` into `C:\Users\<My User>\aem-sdk\dispatcher` （必要に応じて、見つからないフォルダを作成します）
   + macOS/Linux:付属のシェルスクリプトを実行します `aem-sdk-dispatcher-tools-x.x.x-unix.sh` Dispatcher ツールを解凍します。
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

以下で発行されるすべてのコマンドは、現在の作業ディレクトリに拡張されている Dispatcher ツールの内容が含まれていることを前提としています。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*このビデオでは、macOSを参考にしています。 同様の結果を得るために、Windows/Linux の同等のコマンドを使用できます*

## Dispatcher 設定ファイルについて

>[!TIP]
> Experience Managerプロジェクトを [AEM Project Maven アーキタイプ](https://github.com/adobe/aem-project-archetype) は、この一連の Dispatcher 設定ファイルに事前に設定されているので、Dispatcher ツールの src フォルダーからコピーする必要はありません。

Dispatcher ツールは、ローカル開発を含むすべての環境の動作を定義する、Apache HTTP Web サーバーと Dispatcher 設定ファイルのセットを提供します。

これらのファイルは、Experience ManagerMaven プロジェクトの `dispatcher/src` フォルダーに保存します (Experience ManagerMaven プロジェクトにまだ存在しない場合 )。

設定ファイルに関する完全な説明は、展開された Dispatcher ツールでは以下のように表示されます。 `dispatcher-sdk-x.x.x/docs/Config.html`.

## 設定を検証

オプションで、Dispatcher および Apache Web サーバー設定 ( `httpd -t`) は、 `validate` スクリプト ( `validator` 実行可能 )。 この `validate` スクリプトを使用すると、 [3 フェーズ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) の `validator`.

+ 使用方法:
   + Windows：`bin\validate src`
   + macOS/Linux: `./bin/validate.sh ./src`

## Dispatcher をローカルで実行する

AEM Dispatcher は、Docker を使用して、 `src` Dispatcher および Apache Web サーバーの設定ファイル。

+ 使用方法:
   + Windows：`bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

この `<aem-publish-host>` に設定できます。 `host.docker.internal`:Docker がコンテナで提供する、ホストマシンの IP に解決される特別な DNS 名。 もし彼が `host.docker.internal` が解決されない場合は、 [トラブルシューティング](#troubleshooting-host-docker-internal) 」の節を参照してください。

例えば、Dispatcher ツールが提供するデフォルトの設定ファイルを使用して Dispatcher Docker コンテナを起動するには、次のようにします。

Dispatcher 設定の src フォルダーへのパスを指定する、Dispatcher Docker コンテナを起動します。

+ Windows：`bin\docker_run src host.docker.internal:4503 8080`
+ macOS/Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

ポート 4503 でローカルに実行されるAEMas a Cloud ServiceSDK のパブリッシュサービスは、Dispatcher を通じて次の場所で使用できます。 `http://localhost:8080`.

Experience Managerプロジェクトの Dispatcher 設定に対して Dispatcher ツールを実行するには、プロジェクトの `dispatcher/src` フォルダー。

+ Windows：

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher ツールログ

Dispatcher ログは、ローカル開発中に HTTP 要求がブロックされるかどうかとその理由を理解するのに役立ちます。 ログレベルは、 `docker_run` 環境パラメーターを使用します。

Dispatcher ツールのログは、 `docker_run` が実行されます。

Dispatcher のデバッグに役立つパラメーターは次のとおりです。

+ `DISP_LOG_LEVEL=Debug` Dispatcher モジュールのログをデバッグレベルに設定します
   + デフォルト値： `Warn`
+ `REWRITE_LOG_LEVEL=Debug` Apache HTTP Web サーバー書き換えモジュールのログをデバッグレベルに設定します
   + デフォルト値： `Warn`
+ `DISP_RUN_MODE` は、Dispatcher 環境の「実行モード」を設定し、対応する実行モードの Dispatcher 設定ファイルを読み込みます。
   + デフォルトは `dev`
+ 有効な値： `dev`, `stage`または `prod`

1 つ以上のパラメーターをに渡すことができます `docker_run`

+ Windows：

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### ログファイルへのアクセス

Apache Web サーバーとAEM Dispatcher のログは、Docker コンテナで直接アクセスできます。

+ [Docker コンテナ内のログへのアクセス](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Docker ログのローカルファイルシステムへのコピー](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Dispatcher ツールを更新するタイミング{#dispatcher-tools-version}

Dispatcher ツールのバージョンは、Experience Managerよりも増分の頻度が少ないので、ローカル開発環境では、Dispatcher ツールの更新の必要が少なくなります。

推奨される Dispatcher ツールのバージョンは、Experience Managerのas a Cloud Serviceバージョンと一致するAEMas a Cloud ServiceSDK にバンドルされているバージョンです。 AEMas a Cloud Serviceのバージョンは、 [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager /環境__( __AEMリリース__ ラベル

![Experience Managerバージョン](./assets/dispatcher-tools/aem-version.png)

_Dispatcher ツールのバージョン自体は、バージョンのバージョンと一致しないことに注意してください。Experience Managerのバージョンは、_

## トラブルシューティング

### docker_run を実行すると、「host.docker.internal が使用可能になるまで待機中」というメッセージが表示されます。{#troubleshooting-host-docker-internal}

`host.docker.internal` は、ホストに解決されるを含む、Docker に提供されるホスト名です。 docs.docker.com ごと ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Docker 18.03 以降では、ホストが使用する内部 IP アドレスに解決される特別な DNS 名 host.docker.internal に接続することをお勧めします。

場合、 `bin/docker_run src host.docker.internal:4503 8080` 結果はメッセージに含まれます __host.docker.internal が使用可能になるまで待機します__、次に、

1. インストールされている Docker のバージョンが 18.03 以降であることを確認します。
2. ローカルマシンが設定されていて、 `host.docker.internal` 名前。 代わりに、ローカル IP を使用します。
   + Windows：
      + コマンドプロンプトで、を実行します。 `ipconfig`を作成し、ホストの __IPv4 アドレス__ ホストマシンの
      + 次に、次を実行します。 `docker_run` この IP アドレスを使用する場合：
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS/Linux:
      + ターミナルから、次を実行します。 `ifconfig` そしてホストを記録する __inet__ IP アドレス ( 通常は __en0__ デバイス。
      + その後、次を実行 `docker_run` ホスト IP アドレスを使用：
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
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker のダウンロード](https://www.docker.com/)
+ [AEM Reference Web サイト (WKND) のダウンロード](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience ManagerDispatcher のドキュメント](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja)
