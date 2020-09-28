---
title: AEM Sitesの使用の手引き — プロジェクトのセットアップ
seo-title: AEM Sitesの使用の手引き — プロジェクトのセットアップ
description: AEM Sites のコードおよび設定を管理するための、Maven のマルチモジュールプロジェクトの作成について説明します。
sub-product: サイト
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 21%

---


# プロジェクトのセットアップ {#project-setup}

このチュートリアルでは、Adobe Experience Managerサイトのコードと設定を管理するMaven Multi Module Projectの作成について説明します。

## 前提条件 {#prerequisites}

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment). Adobe Experience Managerの新しいインスタンスがローカルで使用されていること、および（必要なService Pack以外の）追加のサンプル/デモパッケージがインストールされていないことを確認します。

## 目的 {#objective}

1. Mavenアーキタイプを使用して新しいAEMプロジェクトを生成する方法を説明します。
1. AEMプロジェクトのアーキタイプで生成される様々なモジュールと、それらの連携の仕組みを理解します。
1. AEMコアコンポーネントがAEMプロジェクトに含まれる方法を理解します。

## 作成する内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

この章では、 [AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成します](https://github.com/adobe/aem-project-archetype)。 AEMプロジェクトには、Sitesの導入に使用されるすべてのコード、コンテンツ、設定が含まれています。 本章で生成するプロジェクトは、WKNDサイトの導入の基盤となり、今後の章で構築される予定です。

## 背景 {#background}

**Mavenプロジェクトとは何ですか？** - [Apache Maven](https://maven.apache.org/) はプロジェクトを構築するためのソフトウェア管理ツールです。 *すべてのAdobe Experience Manager* の導入では、AEM上でカスタムコードを構築、管理、および導入する際に、Mavenプロジェクトを使用します。

**Mavenアーキタイプとは何ですか。** - [Mavenアーキタイプ](https://maven.apache.org/archetype/index.html) は、新しいプロジェクトを生成するためのテンプレートまたはパターンです。 AEMプロジェクトのアーキタイプを使用すると、カスタム名前空間を使用して新しいプロジェクトを生成し、ベストプラクティスに従ったプロジェクト構造を含めることができ、プロジェクトの迅速化に大きく貢献します。

## Create the project {#create}

AEM用のMavenマルチモジュールプロジェクトを作成する方法はいくつかあります。 This tutorial will leverage the [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype). また、Cloud Manager [には、AEMアプリケーションプロジェクトの作成を開始するためのUIウィザード](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) も用意されています。 Cloud Manager UIで生成される基になるプロジェクトの結果は、アーキタイプを直接使用する場合と同じ構造になります。

>[!NOTE]
>
>このチュートリアルの使用目的は、アーキタイプのバージョン22 **を使用してください** 。 ただし、新しいプロジェクトを生成する際は、常に **最新バージョンのアーキタイプを使用することをお勧めします** 。

次の一連の手順は、UNIXベースのコマンド・ライン・ターミナルを使用して行われますが、Windowsターミナルを使用する場合と同じである必要があります。

1. コマンドラインターミナルを開き、Maven がインストールされ、コマンドラインのパスに追加されていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 次のコマンドを実行して、 **adobe-public** プロファイルがアクティブであることを確認します。

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   **adobe-public** が表示されない場合は **、Adobeレポートが**`~/.m2/settings.xml` ファイルで正しく参照されていないことを示しています。 ローカル開発環境でApache Mavenをインストールおよび設定する手順 [に戻ってください](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)。

1. AEMプロジェクトを生成するディレクトリに移動します。 これは、プロジェクトのソースコードを管理する任意のディレクトリにすることができます。 例えば、ユーザーのホームディレクトリ `code` の下にある次の名前のディレクトリがあります。

   ```shell
   $ cd ~/code
   ```

1. 次をコマンドラインに貼り付けて、バッチモードでプロジェクトを [生成します](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)。

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >デフォルトでは、Mavenアーキタイプからプロジェクトを生成する際に、インタラクティブモードが使用されます。 バッチモードを使用して生成した値を、太字運転を避けるため。 It is also possible to create the Maven AEM project using the [AEM Developer Tools plugin for Eclipse](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/aem-eclipse.html).

   >[!CAUTION]
   >
   >次のようなエラーが表示される場合： *プロジェクトstandalone-pomで、目標org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate(default-cli)を実行できませんでした。目的のアーキタイプが存在しません*。 これは、Adobeレポートが `~/.m2/settings.xml` ファイル内で正しく参照されていないことを示しています。 前の手順を再度実行し、settings.xmlファイルがAdobeレポートを参照していることを確認してください。

   次の表は、このチュートリアルで使用される値の一覧です。

   | 名前 | 値 | 説明 |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | Base Maven groupId |
   | artifactId | aem-guides-wknd | ベース Maven ArtifactId |
   | version | 0.0.1-SNAPSHOT | バージョン |
   | package | com.adobe.aem.guides.wknd | Java ソースパッケージ |
   | appsFolderName | wknd | /apps フォルダー名 |
   | artifactName | WKND Sites Project | Maven プロジェクト名 |
   | componentGroupName | WKND | AEM コンポーネントグループ名 |
   | contentFolderName | wknd | /content フォルダー名 |
   | confFolderName | wknd | /conf フォルダー名 |
   | cssId | wknd | 生成された CSS で使用されるプレフィックス |
   | packageGroup | wknd | コンテンツパッケージグループ名 |
   | siteName | WKND Site | AEM サイト名 |
   | optionAemVersion | 6.5.0 | 対象 AEM バージョン |
   | language_country | en_us | 言語/国コード（en_usなど） |
   | optionIncludeExamples | y | コンポーネントライブラリのサンプルサイトを含める |
   | optionIncludeErrorHandler | n | カスタム 404 応答ページを含める |
   | optionIncludeFrontendModule | y | 専用のフロントエンドモジュールを含める |
   | isSingleCountryWebサイト | n | サンプルコンテンツで言語マスター構造を作成する |
   | optionDispatcherConfig | なし | ディスパッチャー設定モジュールの生成 |

1. ローカルファイルシステム上のMavenアーキタイプによって、次のフォルダーとファイル構造が生成されます。

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## プロジェクトの構築 {#build}

新しいプロジェクトが生成されたので、プロジェクトコードをAEMのローカルインスタンスにデプロイできます。

1. AEMのインスタンスがローカルでポート4502で実行されていることを確認 **します**。
1. コマンドラインから、 `aem-guides-wknd` プロジェクトディレクトリに移動します。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 次のコマンドを実行して、プロジェクト全体を構築し、AEMにデプロイします。

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Mavenプロファイルは、プロジェクトの個々のモジュールを `autoInstallSinglePackage` コンパイルし、1つのパッケージをAEMインスタンスにデプロイします。 デフォルトでは、このパッケージは、ローカルでポート4502上で実行され **ているAEMインスタンスに展開され** 、の資格情報が付与され `admin:admin`ます。

1. Navigate to Package Manager on your local AEM instance: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 、、およびの3つのパッケージが表示さ `aem-guides-wknd.ui.apps`れ `aem-guides-wknd.ui.content`るはずで `aem-guides-wknd.all`す。

   また、 [AEMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html) （アーキタイプ別）の複数のパッケージも表示されます。 これについては、チュートリアルで後述します。

1. Navigate to the Sites console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). サイトの 1 つに WKND サイトがあります。このレポートには、米国と言語のマスターの階層を持つサイト構造が含まれます。 このサイト階層は、アーキタイプを使用してプロジェクトを生成す `language_country` る際と生成す `isSingleCountryWebsite` る際の値に基づいています。

1. ページを選択し、メニューバーの **「** 編集 `>` 」ボタンをクリックして、 **米国** 英語 **** ページを開きます。

   ![サイトコンソール](assets/project-setup/aem-sites-console.png)

1. いくつかの作成済みコンテンツおよびページに追加できるコンポーネントがあります。これらのコンポーネントを使用してみることで、機能について大まかに把握できます。このページおよびコンポーネントがどのように設定されているかについては、このチュートリアルの後半で詳細に説明します。

## 計画をInspectする {#project-structure}

AEMアーキタイプは、個々のMavenモジュールで構成されています。

* [コア](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - OSGiサービス、リスナーまたはスケジューラーなどのコア機能のほか、サーブレットや要求フィルターなどのコンポーネント関連のJavaコードが含まれるJavaバンドル。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) — プロジェクトの/apps部分、ie JS&amp;CSSクライアントライブラリ、コンポーネント、およびOSGi設定を含みます。
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) — 編集可能なテンプレート、メタデータスキーマ(/content、/conf)などの構造コンテンツと設定を含みます。
* ui.tests - サーバー側で実行される JUnit テストが含まれる Java バンドル。このバンドルは実稼動環境にはデプロイされません。
* ui.launcher - ui.tests バンドル（および依存するバンドル）をサーバーにデプロイし、JUnit のリモート実行をトリガーするグルーコードが含まれます。
* [ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html) — （オプション）Webpackベースのフロントエンドビルドモジュールを使用するのに必要なアーティファクトが含まれます。
* all — これは空のMavenモジュールで、上記のモジュールを1つのパッケージに組み合わせてAEM環境に展開できます。

![Mavenプロジェクト図](assets/project-setup/project-pom-structure.png)

Mavenモジュールの詳細については、 [AEMプロジェクトのアーキタイプドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html) を参照してください。

## 高度なMavenコマンド {#advanced-maven-commands}

開発中は、モジュールの1つだけで作業を行う場合があり、時間を節約するためにプロジェクト全体を構築しないようにしたいと考えています。 また、AEM発行インスタンスに直接デプロイしたり、ポート4502で実行されていないAEMのインスタンスにデプロイしたりすることもできます。

次に、開発時の柔軟性を高めるために使用できる追加のMavenプロファイルとコマンドをいくつか見てみましょう。

### Core module {#core-module}

コ **[ア](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** モジュールには、プロジェクトに関連付けられたすべてのJavaコードが含まれています。 構築時にOSGiバンドルをAEMにデプロイします。 このモジュールだけを構築するには：

1. フォルダー(下 `core` )に移動し `aem-guides-wknd`ます。

   ```shell
   $ cd core/
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigate to [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). これはOSGi Webコンソールで、AEMインスタンスにインストールされているすべてのバンドルに関する情報が含まれています。

1. 「 **Id** sort」列を切り替えると、WKNDバンドルがインストールされ、アクティブになっていることがわかります。

   ![コアバンドル](assets/project-setup/wknd-osgi-console.png)

1. You can see the &#39;physical&#39; location of the jar in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![CRXDE Location of Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.appsとUi.contentモジュール {#apps-content-module}

The **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven module contains all of the rendering code needed for the site beneath `/apps`. これには CSS／JS が含まれ、それらは [clientlibs](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html) と呼ばれる AEM の形式で保存されます。また、これには動的 HTML をレンダリングするための [HTL](https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html) スクリプトも含まれます。You can think of the **ui.apps** module as a map to the structure in the JCR but in a format that can be stored on a file system and committed to source control. ui.apps **** モジュールにはコードのみが含まれています。

このモジュールを構築するには：

1. コマンドラインから フォルダー(下 `ui.apps` )に移動し `aem-guides-wknd`ます。

   ```shell
   $ cd ../ui.apps
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. http://localhost:4502/crx/packmgr/index.jspに移動し [ます](http://localhost:4502/crx/packmgr/index.jsp)。 最初にインストールされたパッケージとして `ui.apps` パッケージが表示され、他のパッケージよりも新しいタイムスタンプが必要です。

   ![Ui.appsパッケージがインストールされました](assets/project-setup/ui-apps-package.png)

1. コマンドラインに戻り、次のコマンドを実行します( `ui.apps` フォルダ内)。

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   このプロファイル `autoInstallPackagePublish` は、ポート4503で実行されている発行環境にパッケージを展開することを目的としてい **ます**。 http://localhost:4503で実行中のAEMインスタンスが見つからない場合、上記のエラーが発生します。

1. 最後に次のコマンドを実行して、 `ui.apps` パッケージをポート4504に展開し **ます**。

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   ポート4504で実行中のAEMインスタンスが使用できない場合は、再びビルドエラーが発生す **ると考えられます** 。 パラメータ `aem.port` は、のPOMファイルで定義され `aem-guides-wknd/pom.xml`ます。

ui. **[content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** モジュールの構造は、 **ui.apps** モジュールと同じです。 唯一の違いは、 **ui.content** モジュールに可変コンテンツと呼ばれるものが含まれていること **** です。 **可変** コンテンツとは、基本的に、ソース管理に保存されているがAEMインスタンス上で直接変更できる、テンプレート、ポリシー、フォルダー構造などの非コード設定 **を指します** 。 これについては、「ページとテンプレート」の章で詳しく説明します。 現時点で重要な点は、 **ui.apps** モジュールの構築に使用したのと同じMavenコマンドを使用して **ui.content** モジュールを構築できる点です。 上記の手順は、 **ui.content** フォルダー内から自由に繰り返してください。

### Ui.frontend module {#ui-frontend-module}

**[ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html)** モジュールはMavenモジュールで、実際には [Webpack](https://webpack.js.org/) プロジェクトです。 このモジュールは、JavaScriptファイルとCSSファイルを出力し、AEMにデプロイする専用のフロントエンドビルドシステムとして設定されています。 ui.frontend **** モジュールを使用すると、開発者は [Sass](https://sass-lang.com/)、 [TypeScript](https://www.typescriptlang.org/)、npmScript [](https://www.npmjs.com/) を使用し、出力を直接AEMに統合することができます。

ui. **frontend** モジュールの詳細については、クライアント側ライブラリとフロントエンド開発の章で説明します。 ここでは、プロジェクトにどのように統合されているかを見てみましょう。

1. コマンドラインから フォルダー(下 `ui.frontend` )に移動し `aem-guides-wknd`ます。

   ```shell
   $ cd ../ui.frontend
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   線がのように表示され `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`ます。 これは、コンパイルされたCSSとJSが `ui.apps` フォルダーにコピーされることを示します。

1. ファイルの変更されたタイムスタンプの表示 `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`。 モジュール内の他のファイルよりも新しく更新する必要があり `ui.apps` ます。

   他のモジュールとは異なり、 **ui.frontend** モジュールはAEMに直接デプロイすることはできません。 代わりに、CSSとJSが **ui.apps** モジュールにコピーされ、 **ui.apps** モジュールがAEMにデプロイされます。 最初のMavenコマンドのビルド順を見ると、 **ui.frontend** は常に *ui.apps* の前に構築されていることがわかります ****。

   チュートリアルの後半では、 **ui.frontendモジュールと組み込みのwebpack開発サーバの高度な機能を見て、** 迅速な開発を実現します。

## コアコンポーネントを組み込む {#core-components}

アーキタイプは、 [AEMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html) をプロジェクトに自動的に埋め込みます。 以前は、デプロイしたパッケージをAEMに対して検査する場合、コアコンポーネントに関連する複数のパッケージが含まれていました。 コアコンポーネントは、AEM Sites プロジェクトの開発を迅速化するために設計された一連の基本コンポーネントです。コアコンポーネントはオープンソースであり、[GitHub](https://github.com/adobe/aem-core-wcm-components) で利用できます。プロジェクトにコアコンポーネントを [含める方法の詳細については、こちらを参照してください](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)。

1. お気に入りのテキストエディターを開き `aem-guides-wknd/pom.xml`ます。

1. `core.wcm.components.version` を検索。これは、どのバージョンのコアコンポーネントが含まれているかを示します。

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEMプロジェクトのアーキタイプにはAEMコアコンポーネントのバージョンが含まれますが、これらのプロジェクトのリリースサイクルは異なるので、含まれるコアコンポーネントのバージョンは最新ではない場合があります。 ベストプラクティスとして、常に最新バージョンのコアコンポーネントを利用するように検討する必要があります。 新機能とバグ修正は頻繁に更新されます。 最新の [リリース情報はGitHubで確認できます](https://github.com/adobe/aem-core-wcm-components/releases)。

1. この `dependencies` セクションまで下にスクロールすると、個々のコアコンポーネントの依存関係が表示されます。

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## ソース管理システムによる管理 {#source-control}

アプリケーションのコードを管理するために、何らかのソース管理システムを使用することが常に推奨されます。このチュートリアルでは git および GitHub を使用します。Maven などの任意の IDE では、SCM で無視すべきいくつかのファイルが生成されます。

Maven は、コードパッケージをビルドおよびインストールするたびにターゲットフォルダーを作成します。ターゲットフォルダーとコンテンツは、SCMから除外する必要があります。

ui.apps の下に多数の .content.xml ファイルが作成されています。これらの XML ファイルは、JCR にインストールされているコンテンツのノードタイプおよびプロパティをマッピングします。これらのファイルは重要であり、無視し **ないでください** 。

The AEM project archetype will generates a sample `.gitignore` file that can be used as a starting point for which files can be safely ignored. ファイルはに生成され `<src>/aem-guides-wknd/.gitignore`ます。

## レビュー {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## バリデーターが{#congratulations}

初めてのAEMプロジェクトが作成されました。

### 次の手順 {#next-steps}

「基本コンポーネント `HelloWorld` 」チュートリアルの簡単な [](component-basics.md) 例を通して、Adobe Experience Manager(AEM)サイトコンポーネントの基盤となる技術を理解します。
