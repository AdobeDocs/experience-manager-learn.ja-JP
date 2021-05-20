---
title: AEM as a Development用の開発ツールのCloud Service
description: AEMに対してローカルで開発するために必要なすべてのローカル開発ツールを備えたベースラインマシンを設定します。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 4%

---


# 開発ツールの設定

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="開発ツールの設定"
>abstract="Adobe Experience Manager(AEM)の開発では、開発者マシンに最低限の開発ツールのセットをインストールして設定する必要があります。 これらのツールには、Java、Maven、Adobe I/OCLI、Development IDEなどが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開発の基本"

Adobe Experience Manager(AEM)の開発では、開発者マシンに最低限の開発ツールのセットをインストールして設定する必要があります。 これらのツールは、AEM Projectsの開発と構築をサポートします。

`~`は、ユーザーのディレクトリの略記法として使用されます。 Windowsの場合、`%HOMEPATH%`と同じです。

## Javaのインストール

Experience ManagerはJavaCloud Serviceです。したがって、開発をサポートするにはJava SDKが必要で、AEM as a Application SDKとして必要です。

1. [最新リリースのJava 11 SDKをダウンロードしてインストールする](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40cr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDKがインストールされていることを確認します。
   + Windows：`java -version`
   + macOS/Linux:`java --version`

![Java](./assets/development-tools/java.png)

## Homebrewのインストール

_Homebrewの使用は任意ですが、推奨されます。_

Homebrewは、macOS、Windows、Linux用のオープンソースパッケージマネージャーです。 すべてのサポートツールを個別にインストールできます。Homebrewは、Experience Manager開発に必要な様々な開発ツールをインストールおよび更新する便利な方法を提供します。

1. ターミナルを開く
1. 次のコマンドを実行して、Homebrewが既にインストールされているかどうかを確認します。`brew --version`.
1. Homebrewがインストールされていない場合は、Homebrewをインストールします。
   + [macOSでのHomebrewのインストール](https://brew.sh/)
      + macOS上のHomebrewには、次のコマンドを使用して[Xcode](https://apps.apple.com/us/app/xcode/id497799835)または[コマンドラインツール](https://developer.apple.com/download/more/)をインストールできます。
         + `xcode-select --install`
   + [LinuxへのHomebrewのインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Windows 10でのHomebrewのインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 次のコマンドを実行して、Homebrewがインストールされていることを確認します。`brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Homebrewを使用している場合は、以下のセクションの&#x200B;__Homebrew__&#x200B;を使用してインストールする手順に従ってください。 Homebrewを使用して&#x200B;____&#x200B;しない場合は、OS固有のリンクを使用してツールをインストールします。

## Gitのインストール

[](https://git-scm.com/)  [Git](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html)は、AdobeCloud Managerで使用されるソース管理システムなので、開発に必要です。

+ Homebrewを使用したGitのインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。`brew install git`
   1. 次のコマンドを使用して、Gitがインストールされていることを確認します。`git --version`
+ または、Git（macOS、LinuxまたはWindows）をダウンロードしてインストールします。
   1. [Gitをダウンロードしてインストールする](https://git-scm.com/downloads)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Gitがインストールされていることを確認します。`git --version`

![Git](./assets/development-tools/git.png)

## Node.js （およびnpm）{#node-js}のインストール

[Node.](https://nodejs.org) jsは、AEMプロジェクトの __ui.frontendsub-projectのフロントエンドアセットを操作するために使用されるJavaScriptランタイム環境__ です。Node.jsは[npm](https://www.npmjs.com／)と共に配布され、JavaScriptの依存関係の管理に使用される事実上のNode.jsパッケージマネージャーです。

+ Homebrewを使用したNode.jsのインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。`brew install node`
   1. 次のコマンドを使用して、Node.jsがインストールされていることを確認します。`node -v`
   1. 次のコマンドを使用して、npmがインストールされていることを確認します。`npm -v`
+ または、Node.js（macOS、LinuxまたはWindows）をダウンロードしてインストールします。
   1. [Node.jsのダウンロードとインストール](https://nodejs.org/en/download/)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Node.jsがインストールされていることを確認します。`node -v`
   1. 次のコマンドを使用して、npmがインストールされていることを確認します。`npm -v`

![Node.jsとnpm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)ベースのAEMプロジェクトでは、ビルド時に分離バージョンのNode.jsがインストールされます。AEM MavenプロジェクトのReactor pom.xmlで指定されているNode.jsとnpmのバージョンを、ローカル開発システムのバージョンと同期（または近い）に保つことをお勧めします。
>
>Node.jsとnpmのビルドバージョンの場所については、この例[AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118)を参照してください。

## Mavenのインストール

Apache Mavenは、AEMプロジェクトMavenアーキタイプから生成されたAEMプロジェクトを構築するために使用されるオープンソースJavaコマンドラインツールです。 主要なIDE（[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[Visual Studio Code](https://code.visualstudio.com/)、[Eclipse](https://www.eclipse.org/)など） 統合Mavenサポートを持っている。

+ Homebrewを使用したMavenのインストール
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを実行します。`brew install maven`
   1. 次のコマンドを使用して、Mavenがインストールされていることを確認します。`mvn -v`
+ または、Maven（macOS、LinuxまたはWindows）をダウンロードしてインストールします。
   1. [Mavenのダウンロード](https://maven.apache.org/download.cgi)
   1. [Mavenのインストール](https://maven.apache.org/install.html)
   1. ターミナル/コマンドプロンプトを開きます。
   1. 次のコマンドを使用して、Mavenがインストールされていることを確認します。`mvn -v`

![Maven](./assets/development-tools/maven.png)

## Adobe I/OCLI{#aio-cli}の設定

[Adobe I/OCLI](https://github.com/adobe/aio-cli)(`aio`)は、[Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)や[Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute)など、様々なAdobeサービスに対するコマンドラインアクセスを提供します。 Adobe I/OCLIは、開発者に次の機能を提供するので、Cloud ServiceとしてAEMの開発に不可欠です。

+ AEM as a Cloud Servicesサービスからのテールログ
+ CLIからのCloud Managerパイプラインの管理

### Adobe I/OCLI

1. [Node.jsが](#node-js)インストールされていることを確認します。Adobe I/OCLIはnpmモジュールです。
   + `node --version`を実行して確定
1. `npm install -g @adobe/aio-cli`を実行して、`aio` npmモジュールをグローバルにインストールします。

### Adobe I/OCLI Cloud Managerプラグインの設定{#aio-cloud-manager}

Adobe I/OCloud Managerプラグインを使用すると、aio CLIは`aio cloudmanager`コマンドを使用してAdobeCloud Managerとやり取りできます。

1. `aio plugins:install @adobe/aio-cli-plugin-cloudmanager`を実行して、[aio Cloud Managerプラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager)をインストールします。

### Adobe I/OCLIAsset computeプラグイン{#aio-asset-compute}の設定

Adobe I/OCloud Managerプラグインを使用すると、aio CLIで`aio asset-compute`コマンドを使用してAsset computeワーカーを生成し、実行できます。

1. `aio plugins:install @adobe/aio-cli-plugin-asset-compute`を実行して、[aioAsset computeプラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute)をインストールします。

### Adobe I/OCLI認証の設定

Adobe I/OCLIとCloud Managerとの通信をおこなうには、Adobe I/OコンソールでCloud Manager統合を作成し、認証を正常におこなうために資格情報を取得する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. [console.adobe.io](https://console.adobe.io)にログインします。
1. Adobe組織切り替えボタンで、接続先のCloud Manager製品を含む組織がアクティブになっていることを確認します。
1. 新しい[Adobe I/Oプログラムを作成するか、既存の](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)を開きます。
   + Adobe I/Oコンソールプログラムは、統合の管理方法に基づいて、統合、作成または使用および既存のプログラムの単なる組織的なグループです
   + 新しいプロジェクトを作成する場合は、プロンプトが表示されたら「空のプロジェクト」を選択します（テンプレートから作成するのとは異なります）。
   + Adobe I/Oコンソールプログラムは、Cloud Managerプログラムとは異なる概念です
1. 「開発者 —Cloud Service」プロファイルを使用した、新しいCloud Manager API統合の作成
1. Adobe I/OCLIの[config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)に入力する必要があるサービスアカウント(JWT)資格情報を取得します
1. `config.json`ファイルをAdobe I/OCLIに読み込む
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. `private.key`ファイルをAdobe I/OCLIに読み込む
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Adobe I/OCLIを使用して、Cloud Managerの[コマンド](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)の実行を開始します。

## 開発IDEの設定

AEMの開発は、主にJavaおよびフロントエンド（JavaScript、CSSなど）の開発とXML管理で構成されます。 次に、AEM開発で最も一般的なIDEを示します。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ は、Java開発用の強力なIDEです。IntelliJ IDEAは、無料のコミュニティ版と、商用（有料）の究極版の2種類に分かれています。 AEMの開発には無料のコミュニティバージョンで十分ですが、Ultimateの[は機能セット](https://www.jetbrains.com/idea/download)を拡張します。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [IntelliJ IDEAのダウンロード](https://www.jetbrains.com/idea/download)
+ [リポジトリツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)は、フロントエンド開発者向けの無料のオープンソースツールです。Visual Studio Codeは、Adobeツール&#x200B;__[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__&#x200B;を使用して、AEMとのコンテンツ同期を統合するように設定できます。

Visual Studio Codeは、主にフロントエンドコードを作成するフロントエンド開発者に最適な選択肢です。JavaScript、CSSおよびHTML。 VS Codeは[拡張](https://code.visualstudio.com/docs/java/java-tutorial)を介してJavaをサポートしていますが、よりJavaに特有の高度な機能の一部が欠落している可能性があります。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studioコードのダウンロード](https://code.visualstudio.com/Download)
+ [リポジトリツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [aemfed VS Code拡張機能のダウンロード](https://aemfed.io/)
+ [AEM Sync VS Code拡張機能のダウンロード](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ は、Java開発向けの一般的なIDEで、  __[AEM Developer ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsplug-inをサポートし、Adobeが提供する、オーサリング用のIDE内GUIを提供し、JCRコンテンツをローカルAEMインスタンスと同期します。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipseのダウンロード](https://www.eclipse.org/ide/)
+ [Eclipse開発ツールのダウンロード](https://eclipse.adobe.com/aem/dev-tools/)
