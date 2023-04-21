---
title: AEMas a Cloud Service開発用の開発ツールの設定
description: AEMに対してローカルで開発するために必要なすべてのローカル開発ツールを備えたベースラインマシンを設定します。
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 9%

---

# 開発ツールの設定 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="開発ツールの設定"
>abstract="Adobe Experience Manager (AEM) を開発する際には、開発用マシンに最低限の開発ツールのセットをインストールして設定する必要があります。これらのツールには、Java、Maven、Adobe I/O CLI、開発 IDE などが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=ja" text="開発の基本"

Adobe Experience Manager (AEM) を開発する際には、開発用マシンに最低限の開発ツールのセットをインストールして設定する必要があります。これらのツールは、AEM Projects の開発と構築をサポートします。

注意： `~` は、ユーザーのディレクトリの略記法として使用されます。 Windows の場合、これは `%HOMEPATH%`.

## Java のインストール

Experience Managerは Java アプリケーションなので、開発とAEMas a Cloud ServiceSDK をサポートするには Java SDK が必要です。

1. [最新リリースの Java 11 SDK をダウンロードしてインストールする](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDK がインストールされていることを確認します。
   + Windows：`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Homebrew のインストール

_Homebrew の使用はオプションですが、推奨されます。_

Homebrew は、macOS、Windows、および Linux 用のオープンソースパッケージマネージャーです。 すべてのサポートツールを個別にインストールできます。Homebrew は、Experience Manager開発に必要な様々な開発ツールをインストールおよび更新する便利な方法を提供します。

1. ターミナルを開く
1. 次のコマンドを実行して、Homebrew が既にインストールされているかどうかを確認します。 `brew --version`.
1. Homebrew がインストールされていない場合は、Homebrew をインストールします。
   + [Homebrew をmacOSにインストール](https://brew.sh/)
      + macOSの Homebrew は [Xcode](https://apps.apple.com/us/app/xcode/id497799835) または [コマンドラインツール](https://developer.apple.com/download/more/)、次のコマンドを使用して install-able を実行します。
         + `xcode-select --install`
   + [Linux での Homebrew のインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Windows 10 での Homebrew のインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 次のコマンドを実行して、Homebrew がインストールされていることを確認します。 `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Homebrew を使用している場合は、 __Homebrew を使用してインストール__ 以下の節の手順を参照してください。 次の場合、 __not__ Homebrew を使用し、OS 固有のリンクを使用してツールをインストールします。

## Git のインストール

[Git](https://git-scm.com/) は、 [AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)の場合は、開発に必要になります。

+ Homebrew を使用した Git のインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。 `brew install git`
   1. 次のコマンドを使用して、Git がインストールされていることを確認します。 `git --version`
+ または、Git(macOS、Linux、Windows) をダウンロードしてインストールします。
   1. [Git をダウンロードしてインストールする](https://git-scm.com/downloads)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Git がインストールされていることを確認します。 `git --version`

![Git](./assets/development-tools/git.png)

## Node.js （および npm）のインストール{#node-js}

[Node.js](https://nodejs.org) は、AEMプロジェクトのフロントエンドアセットを操作するために使用される JavaScript ランタイム環境です。 __ui.frontend__ サブプロジェクト。 Node.js は [npm](https://www.npmjs.com／)は、JavaScript の依存関係の管理に使用される、事実上の Node.js パッケージマネージャーです。

+ Homebrew を使用した Node.js のインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。 `brew install node`
   1. 次のコマンドを使用して、Node.js がインストールされていることを確認します。 `node -v`
   1. 次のコマンドを使用して、npm がインストールされていることを確認します。 `npm -v`
+ または、Node.js(macOS、Linux、Windows) をダウンロードしてインストールします。
   1. [Node.js をダウンロードしてインストールする](https://nodejs.org/en/download/)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Node.js がインストールされていることを確認します。 `node -v`
   1. 次のコマンドを使用して、npm がインストールされていることを確認します。 `npm -v`

![Node.js と npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)ベースのAEM Projects では、ビルド時に独立したバージョンの Node.js がインストールされます。 ローカル開発システムのバージョンを、AEM Maven プロジェクトの pom.xml で指定された Node.js バージョンと npm バージョンとの同期（または近接）に保つことをお勧めします。
>
>次の例を参照してください。 [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) :Node.js および npm のビルドバージョンの場所。

## Maven のインストール

Apache Maven は、AEM Project Maven アーキタイプから生成されたAEMプロジェクトを構築するために使用されるオープンソースの Java コマンドラインツールです。 すべての主要な IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/)など ) は、Maven と統合されたサポートを持っています。

+ Homebrew を使用した Maven のインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。 `brew install maven`
   1. 次のコマンドを使用して、Maven がインストールされていることを確認します。 `mvn -v`
+ または、Maven(macOS、Linux、Windows) をダウンロードしてインストールします。
   1. [Maven のダウンロード](https://maven.apache.org/download.cgi)
   1. [Maven のインストール](https://maven.apache.org/install.html)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Maven がインストールされていることを確認します。 `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Adobe I/OCLI の設定{#aio-cli}

この [Adobe I/OCLI](https://github.com/adobe/aio-cli)または `aio`は、次のような様々なAdobe サービスにコマンドラインでアクセスできます。 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) および [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/OCLI は、AEM as a Cloud Serviceの開発に不可欠な役割を果たし、開発者に次の機能を提供します。

+ AEM as a Cloud Servicesサービスからのテールログ
+ CLI からの Cloud Manager パイプラインの管理
+ デプロイ先 [AEMの迅速な開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=ja)

### Adobe I/OCLI のインストール

1. 確認 [Node.js がインストールされている](#node-js) Adobe I/OCLI は npm モジュールであるため
   + 実行 `node --version` 確認する
1. 実行 `npm install -g @adobe/aio-cli` をインストールするには、 `aio` npm モジュールグローバル

### Adobe I/OCLI Cloud Manager プラグインの設定{#aio-cloud-manager}

Adobe I/OCloud Manager プラグインを使用すると、aio CLI は、 `aio cloudmanager` コマンドを使用します。

1. 実行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` をインストールするには、 [aio Cloud Manager プラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Adobe I/OCLI 認証の設定

Adobe I/OCLI が Cloud Manager と通信するために、 [Cloud Manager 統合は、Adobe I/Oコンソールで作成する必要があります](https://github.com/adobe/aio-cli-plugin-cloudmanager)認証を正常におこなうには、および資格情報を取得する必要があります。

1. にログインします。 [console.adobe.io](https://console.adobe.io)
1. 接続先の Cloud Manager 製品を含む組織が組織組織切り替えボタンでアクティブになっていることをAdobeします。
1. 新規を作成するか、既存の [Adobe I/O計画](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/Oコンソールプログラムは、統合の管理方法に基づいて、統合、作成または使用および既存のプログラムの単なる組織的なグループです
   + 新しいプロジェクトを作成する場合は、プロンプトが表示されたら「空のプロジェクト」を選択します (&quot;テンプレートから作成&quot;)
   + Adobe I/Oコンソールプログラムは、Cloud Manager プログラムとは異なる概念です
1. 「開発者 —Cloud Service」プロファイルを使用して、新しい Cloud Manager API 統合を作成する
1. Adobe I/OCLI の [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. を読み込む `config.json` ファイルをAdobe I/OCLI に
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. を読み込む `private.key` ファイルをAdobe I/OCLI に
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

開始 [コマンドの実行](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) Adobe I/OCLI を介して Cloud Manager の

### AEM Rapid Development Environment プラグインの設定{#rde}

AEM Rapid Development Environment プラグインを使用すると、aio CLI とAEM as a Cloud Serviceの間のやり取りが可能になります [迅速な開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=ja) 経由 `aio aem:rde` コマンドを使用します。

1. 実行 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` をインストールするには、 [AEM Rapid Development Environments プラグイン](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Adobe I/OCLIAsset computeプラグインの設定{#aio-asset-compute}

Adobe I/OCloud Manager プラグインを使用すると、aio CLI で、 `aio asset-compute` コマンドを使用します。

1. 実行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` をインストールするには、 [aioAsset computeプラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute).


## 開発 IDE の設定

AEMの開発は、主に Java とフロントエンド（JavaScript、CSS など）の開発と XML 管理で構成されます。 次に、AEM開発で最も一般的な IDE を示します。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ は、Java 開発用の強力な IDE です。 IntelliJ IDEA は、無料のコミュニティ版と商用（有料）の Ultimate 版の 2 種類のフレーバーで提供されます。 AEM開発には無料のコミュニティバージョンで十分ですが、Ultimate [機能セットを展開します。](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [IntelliJ IDEA のダウンロード](https://www.jetbrains.com/idea/download)
+ [リポジトリツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) は、フロントエンド開発者向けの無料のオープンソースツールです。 Visual Studio Code は、コンテンツ同期をAEMと統合するように、Adobeツールの助けを借りて設定できます。 __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code は、主にフロントエンドコードを作成するフロントエンド開発者に最適な選択肢です。JavaScript、CSS およびHTML。 VS Code は [拡張機能](https://code.visualstudio.com/docs/java/java-tutorial)の場合は、より Java に特有ので提供される高度な機能の一部が欠落している可能性があります。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio コードのダウンロード](https://code.visualstudio.com/Download)
+ [リポジトリツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [aemfed VS Code 拡張機能をダウンロードします。](https://aemfed.io/)
+ [AEM Sync VS Code 拡張機能のダウンロード](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ は、Java 開発用の一般的な IDE で、  __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ Adobeが提供するプラグイン。JCR コンテンツをローカルAEMインスタンスと同期するための IDE 内 GUI を提供します。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse をダウンロード](https://www.eclipse.org/ide/)
+ [Eclipse 開発ツールのダウンロード](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
