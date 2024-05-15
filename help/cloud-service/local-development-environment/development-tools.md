---
title: AEM as a Cloud Service 開発用の開発ツールの設定
description: AEM に対してローカルで開発するために必要なすべてのベースラインツールを備えたローカル開発マシンを設定します。
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3508
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 100%

---

# 開発ツールの設定 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="開発ツールの設定"
>abstract="Adobe Experience Manager（AEM）開発では、最小限の開発ツールセットを開発者マシンにインストールして設定する必要があります。これらのツールには、Java、Maven、Adobe I/O CLI、開発 IDE などが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=ja" text="開発の基本"

Adobe Experience Manager（AEM）開発では、最小限の開発ツールセットを開発者マシンにインストールして設定する必要があります。これらのツールは、AEM プロジェクトの開発と作成をサポートします。

`~` は、ユーザーのディレクトリの略記法として使用されます。Windows では、これは `%HOMEPATH%` に相当します。

## Java のインストール

Experience Manager は Java アプリケーションなので、開発と AEM as a Cloud Service SDK をサポートするには Java SDK が必要です。

1. [最新リリースの Java 11 SDK をダウンロードしてインストールする](https://experience.adobe.com/#/downloads/content/software-distribution/jp/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Oracle Java 11 SDK がインストールされていることを確認します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Homebrew のインストール

_Homebrew の使用は必須ではありませんが、推奨されます。_

Homebrew は、macOS、Windows および Linux 用のオープンソースパッケージマネージャーです。 すべてのサポートツールを個別にインストールできます。Homebrew では、Experience Manager 開発に必要な各種の開発ツールを簡単にインストールおよび更新できます。

1. ターミナルを開く
1. 次のコマンドを実行して、Homebrew が既にインストールされているかどうかを確認します。 `brew --version`
1. Homebrew がインストールされていない場合は、Homebrew をインストールします。

>[!BEGINTABS]

>[!TAB macOS]

[](https://brew.sh/)macOS で Homebrew を使用する場合、[Xcode](https://apps.apple.com/us/app/xcode/id497799835) または[コマンドラインツール](https://developer.apple.com/download/more/)が必要です。このツールは次のコマンドでインストールできます。

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Windows 10 Homebrew をインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Linux で Homebrew をインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. 次のコマンドを実行して、Homebrew がインストールされていることを確認します。 `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Homebrew を使用している場合は、以下の __Homebrew を使用してインストール__&#x200B;の節の手順を参照してください。 Homebrew を使用して&#x200B;__いない__&#x200B;場合は、OS 専用のリンクを使用してツールをインストールします。

## Git のインストール

[Git](https://git-scm.com/) は [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=ja) で使用されるソース管理システムで、開発に必要となります。

>[!BEGINTABS]

>[!TAB Homebrew を使用した Git のインストール]

1. ターミナル／コマンドプロンプトを開きます。
1. 次のコマンドを実行します。 `$ brew install git`
1. 次のコマンドを使用して、Git がインストールされていることを確認します。 `$ git --version`

>[!TAB Git をダウンロードしてインストールする]

1. [Git をダウンロードしてインストールする](https://git-scm.com/downloads)
1. ターミナル／コマンドプロンプトを開きます。
1. 次のコマンドを使用して、Git がインストールされていることを確認します。`$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Node.js （および npm）のインストール{#node-js}

[Node.js](https://nodejs.org) は、AEM プロジェクトの __ui.frontend__ サブプロジェクトのフロントエンドアセットを操作するために使用される JavaScript ランタイム環境です。Node.js は [npm](https://www.npmjs.com/) と一緒に配布される、事実上の Node.js パッケージマネージャーで、JavaScript の依存関係の管理に使用されます。

>[!BEGINTABS]

>[!TAB Homebrew を使用して Node.js をインストールする]

1. ターミナル／コマンドプロンプトを開きます。
1. 次のコマンドを実行します。`$ brew install node`
1. 次のコマンドを使用して、Node.js がインストールされていることを確認します。`$ node -v`
1. 次のコマンドを使用して、npm がインストールされていることを確認します。`$ npm -v`

>[!TAB Node.js をダウンロードしてインストールする]

1. [Node.js をダウンロードしてインストールする](https://nodejs.org/ja/download/)
2. ターミナル／コマンドプロンプトを開きます。
3. 次のコマンドを使用して、Node.js がインストールされていることを確認します。`$ node -v`
4. 次のコマンドを使用して、npm がインストールされていることを確認します。`$ npm -v`

>[!ENDTABS]

![Node.js と npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)ベースの AEM プロジェクトでは、独立したバージョンの Node.js がビルド時にインストールされます。 ローカル開発システムのバージョンを、AEM Maven プロジェクトのリアクター pom.xml で指定された Node.js バージョンと npm バージョンと同期する（またはそれに近い状態に保つ）ことをお勧めします。
>
>Node.js および npm のビルドバージョンの場所については、[AEM プロジェクトのリアクター pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) の例を参照してください。

## Maven のインストール

Apache Maven は、AEM プロジェクトの Maven アーキタイプから生成された AEM プロジェクトを作成するために使用される、オープンソースの Java コマンドラインツールです。 すべての主要な IDE（[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[Visual Studio Code](https://code.visualstudio.com/)、[Eclipse](https://www.eclipse.org/) など ）に、Maven のサポートが統合されています。


>[!BEGINTABS]

>[!TAB Homebrew を使用して Maven をインストールする]

1. ターミナル／コマンドプロンプトを開きます。
1. 次のコマンドを実行します。`$ brew install maven`
1. 次のコマンドを使用して、Maven がインストールされていることを確認します。`$ mvn -v`

>[!TAB Maven をダウンロードしてインストールする]

1. [Maven のダウンロード](https://maven.apache.org/download.cgi)
1. [Maven のインストール](https://maven.apache.org/install.html)
1. ターミナル／コマンドプロンプトを開きます。
1. Maven がインストールされていることを次のコマンドを使用して確認します。`$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Adobe I/O CLI の設定{#aio-cli}

[Adobe I/O CLI](https://github.com/adobe/aio-cli)（`aio`）を使用すると、[Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) や [Asset Compute などの、様々な Adobe サービスにコマンドラインでアクセスできます](https://github.com/adobe/aio-cli-plugin-asset-compute)。Adobe I/O CLI は、AEM as a Cloud Service での開発において不可欠な役割を果たし、開発者に次の機能を提供します。

+ AEM as a Cloud Services サービスからのログの確認
+ CLI からの Cloud Manager パイプラインの管理
+ [AEM の迅速な開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=ja)へのデプロイ

### Adobe I/O CLI のインストール

1. Adobe I/O CLI は npm モジュールであるため、[Node.js がインストールされていること](#node-js)を確認します。
   + `node --version` を実行して確認する
1. `npm install -g @adobe/aio-cli` を実行して `aio` npm モジュールをグローバルにインストールする

### Adobe I/O CLI Cloud Manager プラグインの設定{#aio-cloud-manager}

Adobe I/O Cloud Manager プラグインを使用すると、aio CLI で `aio cloudmanager` コマンドを通じて Adobe Cloud Manager とやり取りできます。

1. `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` を実行して、[aio Cloud Manager プラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager)をインストールします。

#### Adobe I/O CLI 認証の設定

Adobe I/O CLI が Cloud Manager と通信するには、[Adobe I/O コンソールで Cloud Manager 統合を作成](https://github.com/adobe/aio-cli-plugin-cloudmanager)し、正常に認証するために資格情報を取得する必要があります。

1. [console.adobe.io](https://console.adobe.io) にログインする
1. 接続先の Cloud Manager 製品を含む組織が Adobe 組織スイッチャーでアクティブになっていることを確認する
1. 既存の [Adobe I/O プログラム](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)を開くまたは新規作成する
   + Adobe I/O コンソールプロジェクトは統合を組織的にグループ化したものであり、統合を管理する方法に基づいて、プロジェクトを作成または既存のものを使用します。
   + 新規プロジェクトを作成する場合は、プロンプトが表示されたら「空のプロジェクト」を選択します（「テンプレートから作成」は選択しない）。
   + Adobe I/O コンソールプログラムは、Cloud Manager プログラムと概念が異なります。
1. 新しい Cloud Manager API 統合を作成する
   + 非推奨の「サービスアカウント (JWT)」認証タイプを選択します（現時点では、CLI で OAuth がサポートされていません）。
   + キーを作成またはアップロードします。
   + 「開発者 - Cloud Service」製品プロファイルを選択します
1. Adobe I/O CLI の [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) に入力するために必要なサービスアカウント（JWT）資格情報を取得する

   ```json
   //config.json 
   {
      "client_id": "Client ID from Service Account (JWT) credential",
      "client_secret": "Client Secret from Service Account (JWT) credential",
      "technical_account_id": "Technical Account ID from Service Account (JWT) credential",
      "ims_org_id": "Organization ID from Service Account (JWT) credential",
      "meta_scopes": [
        "ent_cloudmgr_sdk"
      ]
   }
   ```

1. `config.json` ファイルを Adobe I/O CLI に読み込む
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager ./path/to/config.json --file --json`
1. `private.key` ファイルを Adobe I/O CLI に読み込む
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key ./path/to/private.key --file`

Adobe I/O CLI を使用して Cloud Manager の[コマンドの実行](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)を開始します。

### AEM の迅速な開発環境プラグインのセットアップ{#rde}

AEM の迅速な開発環境プラグインを使用すると、aio CLI が `aio aem:rde` コマンドで、AEM as a Cloud Service の[迅速な開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=ja)とやり取りできるようになります。

1. `aio plugins:install @adobe/aio-cli-plugin-aem-rde` を実行して、[AEM の迅速な開発環境プラグイン](https://github.com/adobe/aio-cli-plugin-aem-rde)をインストールします。

### Adobe I/O CLI Asset Compute プラグインの設定{#aio-asset-compute}

Adobe I/O Cloud Manager プラグインを使用すると、aio CLI で `aio asset-compute` コマンドを使用して Asset Compute ワーカーを生成および実行できます。

1. `aio plugins:install @adobe/aio-cli-plugin-asset-compute` を実行して、[aio Asset Compute プラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute)をインストールします。

## 開発用 IDE の設定

AEM 開発は、主に Java やフロントエンド（JavaScript、CSS など）の開発と XML 管理で構成されます。AEM 開発で最も一般的な IDE を次に示します。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ は、Java 開発用の強力な IDE です。IntelliJ IDEA には、無料の Community 版と商用（有料）の Ultimate 版の 2 種類があります。AEM 開発には無料の Community 版でも十分ですが、Ultimate 版では[さらに拡張された一連の機能を利用できます](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [IntelliJ IDEA のダウンロード](https://www.jetbrains.com/idea/download)
+ [リポジトリツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__（VS Code）は、フロントエンド開発者向けの無料のオープンソースツールです。Visual Studio Code は、アドビのツール、__[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__ を使用してコンテンツ同期を AEM と統合するように設定できます。

Visual Studio Code は、主に JavaScript、CSS、HTML などのフロントエンドコードを作成するフロントエンド開発者に最適な選択肢です。VS Code は[拡張機能](https://code.visualstudio.com/docs/java/java-tutorial)を介して Java をサポートしていますが、Java 特有の拡張機能で提供される高度な機能の一部が欠落している可能性があります。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio Code のダウンロード](https://code.visualstudio.com/Download)
+ [Repo ツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [aemfed VS Code 拡張機能のダウンロード](https://aemfed.io/)
+ [AEM Sync VS Code 拡張機能のダウンロード](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ は、Java 開発用の一般的な IDE で、Adobeが提供する __[AEM 開発者ツール](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=ja)__ プラグインをサポートしており、オーサリング用の IDE 内 GUI を提供し、JCR コンテンツをローカル AEM インスタンスと同期します。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse のダウンロード](https://www.eclipse.org/ide/)
+ [Eclipse 開発ツールのダウンロード](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=ja)
