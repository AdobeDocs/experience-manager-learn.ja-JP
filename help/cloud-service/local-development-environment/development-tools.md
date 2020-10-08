---
title: Cloud Service開発としてAEM用の開発ツールを設定する
description: AEMに対してローカルで開発するために必要なすべてのベースラインツールを備えたローカル開発マシンをセットアップします。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: cb5f3c323c433c9321ba26ac1194be0cd225a405
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 3%

---


# 開発ツールのセットアップ

Adobe Experience Manager(AEM)の開発には、開発者用マシンに最小限の開発ツールをインストールしてセットアップする必要があります。 これらのツールは、AEMプロジェクトの開発と構築をサポートしています。

は、ユーザーのディレクトリの略記法 `~` として使用されます。 Windowsでは、これはと同じで `%HOMEPATH%`す。

## Javaのインストール

Experience ManagerはJavaアプリケーションなので、開発をサポートするJava SDKと、AEMSDKとしてのCloud Serviceが必要です。

1. [最新リリースのJava 11 SDKをダウンロードしてインストールします](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3Content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=リスト&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDKがインストールされていることを確認します。
   + Windows：`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Homebrewのインストール

_Homebrewの使用は任意ですが、推奨されます。_

Homebrewは、macOS、Windows、およびLinux用のオープンソースパッケージマネージャです。 すべてのサポートツールを個別にインストールできます。Homebrewは、Experience Manager開発に必要な様々な開発ツールをインストールして更新する便利な方法です。

1. ターミナルを開く
1. 次のコマンドを実行して、Homebrewが既にインストールされているかどうかを確認します。 `brew --version`.
1. Homebrewがインストールされていない場合は、Homebrewをインストールします
   + [macOSでのHomebrewのインストール](https://brew.sh/)
      + macOS上のHomebrewには、 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) または [](https://developer.apple.com/download/more/)コマンドラインツールが必要です。これらのツールは、次のコマンドを使用してインストールできます。
         + `xcode-select --install`
   + [LinuxへのHomebrewのインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Windows 10でのHomebrewのインストール](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 次のコマンドを実行して、Homebrewがインストールされていることを確認します。 `brew --version`

![自家製](./assets/development-tools/homebrew.png)

Homebrewを使用している場合は、次のセクションの「Homebrewを使用した __インストール__ 」の手順に従ってください。 Homebrewを使用し __ない場合__ 、OS固有のリンクを使用してツールをインストールします。

## Gitのインストール

[Git](https://git-scm.com/) は [Adobeクラウドマネージャーで使用されるソース管理システムです](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html)。このため、開発に必要です。

+ Homebrewを使用したGitのインストール
   1. ターミナル/コマンドプロンプトを開く
   1. コマンドを実行します。 `brew install git`
   1. 次のコマンドを使用して、Gitがインストールされていることを確認します。 `git --version`
+ または、Gitをダウンロードしてインストールします（macOS、LinuxまたはWindows）
   1. [Gitのダウンロードとインストール](https://git-scm.com/downloads)
   1. ターミナル/コマンドプロンプトを開く
   1. 次のコマンドを使用して、Gitがインストールされていることを確認します。 `git --version`

![Git](./assets/development-tools/git.png)

## Node.js（およびnpm）のインストール{#node-js}

[Node.js](https://nodejs.org) は、AEMプロジェクトの ____ ui.frontendサブプロジェクトのフロントエンドアセットを操作するために使用されるJavaScriptランタイム環境です。 Node.jsは [npmと共に配布されます](https://www.npmjs.com/)。これは、JavaScriptの依存関係の管理に使用される、事実上のNode.jsパッケージマネージャーです。

+ Homebrewを使用したNode.jsのインストール
   1. ターミナル/コマンドプロンプトを開く
   1. コマンドを実行します。 `brew install node`
   1. 次のコマンドを使用して、Node.jsがインストールされていることを確認します。 `node -v`
   1. npmがインストールされていることを確認するには、次のコマンドを使用します。 `npm -v`
+ または、Node.jsをダウンロードしてインストールします（macOS、LinuxまたはWindows）。
   1. [Node.jsのダウンロードとインストール](https://nodejs.org/en/download/)
   1. ターミナル/コマンドプロンプトを開く
   1. 次のコマンドを使用して、Node.jsがインストールされていることを確認します。 `node -v`
   1. npmがインストールされていることを確認するには、次のコマンドを使用します。 `npm -v`

![Node.jsとnpm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)ベースのAEMプロジェクトは、ビルド時にNode.jsの独立したバージョンをインストールします。 ローカル開発システムのバージョンをAEM MavenプロジェクトのReactor pom.xmlで指定されたNode.jsとnpmのバージョンと同期（または近接）させておくことをお勧めします。
>
>Node.jsとnpmのビルドバージョンの場所については、次の [例のAEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) を参照してください。

## Mavenのインストール

Apache Mavenは、AEM Project Mavenアーキタイプから生成されたAEMプロジェクトを構築するために使用される、オープンソースのJavaコマンドラインツールです。 主要なIDE([IntelliJ IDEA](https://www.jetbrains.com/idea/)、 [Visual Studioコード](https://code.visualstudio.com/)、 [Eclipse](https://www.eclipse.org/)など) 統合されたMavenのサポートがあります。

+ Homebrewを使用したMavenのインストール
   1. ターミナル/コマンドプロンプトを開く
   1. コマンドを実行します。 `brew install maven`
   1. 次のコマンドを使用して、Mavenがインストールされていることを確認します。 `mvn -v`
+ または、Maven（macOS、LinuxまたはWindows）をダウンロードしてインストールします。
   1. [Mavenのダウンロード](https://maven.apache.org/download.cgi)
   1. [Mavenのインストール](https://maven.apache.org/install.html)
   1. ターミナル/コマンドプロンプトを開く
   1. 次のコマンドを使用して、Mavenがインストールされていることを確認します。 `mvn -v`

![Maven](./assets/development-tools/maven.png)

## AdobeI/O CLIのセットアップ{#aio-cli}

[AdobeI/O CLI](https://github.com/adobe/aio-cli)(または `aio`)は、 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) や [Asset Computeなど、様々なAdobeサービスに対するコマンドラインアクセスを提供します](https://github.com/adobe/aio-cli-plugin-asset-compute)。 AdobeI/O CLIは、開発者に次の機能を提供するため、AEMのCloud Serviceとしての開発に不可欠です。

+ Cloud ServicesサービスとしてのAEMからの末尾ログ
+ CLIからのCloud Managerパイプラインの管理

### AdobeI/O CLIのインストール

1. Adobe [I/O CLIがnpmモジュールであるため](#node-js) 、Node.jsがインストールされていることを確認する
   + 実行 `node --version` して確認
1. を実行 `npm install -g @adobe/aio-cli``aio` して、npmモジュールをグローバルにインストールします。

### AdobeI/O CLI Cloud Managerプラグインのセットアップ{#aio-cloud-manager}

AdobeI/O Cloud Managerプラグインを使用すると、aio CLIとAdobeCloud Managerとのやり取りが `aio cloudmanager` コマンドを使用して行えます。

1. Aio Cloud Managerプラグイン `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` のインスト [ールを実行します](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### AdobeI/O CLIのAsset Computeプラグインの設定{#aio-asset-compute}

AdobeI/O Cloud Managerプラグインを使用すると、aio CLIで `aio asset-compute` コマンドを使用してアセットコンピューティングワーカーを生成し、実行できます。

1. を実行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` して、 [aio Asset Computeプラグインをインストールし](https://github.com/adobe/aio-cli-plugin-asset-compute)ます。

### AdobeI/O CLI認証の設定

AdobeI/O CLIがCloud Managerと通信するには、AdobeI/OコンソールでCloud Manager統合を作成し、認証を正常に行うために資格情報を取得する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. [console.adobe.ioにログインします。](https://console.adobe.io)
1. Adobe組織の切り替えボタンで、接続先のCloud Manager製品を含む組織がアクティブであることを確認します。
1. 新規作成するか、既存の [AdobeI/Oプログラムを開く](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + AdobeI/Oコンソールプログラムは、統合の管理方法に基づいて、プログラム、作成、使用、既存統合を組織的にグループ化したものです
   + 新しいプロジェクトを作成する場合、指示に従って「空のプロジェクト」を選択します（「テンプレートから作成」と比較）。
   + AdobeI/Oコンソールのプログラムは、Cloud Managerプログラムとは異なる概念です
1. 「開発者 —Cloud Service」プロファイルとの新しいCloud Manager API統合の作成
1. AdobeI/O CLIの [config.jsonに入力する必要があるサービスアカウント(JWT)資格情報を取得します](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. AdobeI/O CLIに `config.json` ファイルをロードする
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. AdobeI/O CLIに `private.key` ファイルをロードする
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

AdobeI/O CLIを使用して [、Cloud Managerのコマンドの](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) 実行を開始します。

## 開発IDEの設定

AEMの開発は、主に、Javaとフロントエンド（JavaScript、CSSなど）の開発およびXML管理で構成されます。 AEM開発で最も一般的なIDEは次のとおりです。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ はJava開発用の強力なIDEです。 IntelliJ IDEAには、無料コミュニティ版と商用（有料） Ultimate版の2つの種類があります。 無料のコミュニティバージョンはAEM開発には十分ですが、Ultimateは機能セットを [拡張しています](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [IntelliJ IDEAのダウンロード](https://www.jetbrains.com/idea/download)
+ [レポートツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studioコード

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)は、フロントエンド開発者向けの無料のオープンソースツールです。 Visual Studioコードは、Adobeツール( __[レポート](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__)の助けを借りて、AEMとのコンテンツ同期の統合を設定できます。

Visual Studioコードは、フロントエンド開発者が主にフロントエンドコードを作成する場合に最適な選択肢です。JavaScript、CSS、HTML。 VSコードは [拡張機能を介してJavaをサポートしていますが](https://code.visualstudio.com/docs/java/java-tutorial)、Java固有の高度な機能の一部が欠けている場合があります。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studioコードのダウンロード](https://code.visualstudio.com/Download)
+ [レポートツールのダウンロード](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [埋め込みVSコード拡張子のダウンロード](https://aemfed.io/)
+ [AEM Sync VSコード拡張のダウンロード](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ はJava開発のための一般的なIDEで、Adobeが提供する __[AEM Developer Tools](https://eclipse.adobe.com/aem/dev-tools/)__ プラグインをサポートし、JCRコンテンツをオーサリングし、ローカルのAEMインスタンスと同期するためのIDE内GUIを提供します。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipseのダウンロード](https://www.eclipse.org/ide/)
+ [Eclipse開発ツールのダウンロード](https://eclipse.adobe.com/aem/dev-tools/)
