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

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 Adobe Experience Managerの新しいインスタンスがローカルで使用されていること、および（必要なService Pack以外の）追加のサンプル/デモパッケージがインストールされていないことを確認します。

## 目的 {#objective}

1. Mavenアーキタイプを使用して新しいAEMプロジェクトを生成する方法を説明します。
1. AEMプロジェクトのアーキタイプで生成される様々なモジュールと、それらの連携の仕組みを理解します。
1. AEMコアコンポーネントがAEMプロジェクトに含まれる方法を理解します。

## 作成する内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

この章では、[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用して、新しいAdobe Experience Managerプロジェクトを生成します。 AEMプロジェクトには、Sitesの導入に使用されるすべてのコード、コンテンツ、設定が含まれています。 本章で生成するプロジェクトは、WKNDサイトの導入の基盤となり、今後の章で構築される予定です。

## 背景 {#background}

**Mavenプロジェクトとは何ですか？** -  [Apache ](https://maven.apache.org/) Mavenisはプロジェクトを構築するためのソフトウェア管理ツールです。*すべてのAdobe Experience* Managerの実装では、Mavenプロジェクトを使用して、カスタムコードをAEM上で構築、管理およびデプロイします。

**Mavenアーキタイプとは何ですか。** -  [](https://maven.apache.org/archetype/index.html) Mavenアーキタイプは、新しいプロジェクトを生成するためのテンプレートまたはパターンです。AEMプロジェクトのアーキタイプを使用すると、カスタム名前空間を使用して新しいプロジェクトを生成し、ベストプラクティスに従ったプロジェクト構造を含めることができ、プロジェクトの迅速化に大きく貢献します。

## プロジェクト{#create}を作成

AEM用のMavenマルチモジュールプロジェクトを作成する方法はいくつかあります。 このチュートリアルでは、[Maven AEMプロジェクトのアーキタイプ&#x200B;**22**](https://github.com/adobe/aem-project-archetype)を活用します。 また、Cloud Managerには、AEMアプリケーションプロジェクトの作成を開始するためのUIウィザード[が用意されています。 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html)Cloud Manager UIで生成される基になるプロジェクトの結果は、アーキタイプを直接使用する場合と同じ構造になります。

>[!NOTE]
>
>このチュートリアルの使用目的は、アーキタイプのバージョン&#x200B;**22**&#x200B;を使用してください。 ただし、新しいプロジェクトを生成する際には、常に、アーキタイプの&#x200B;**最新**&#x200B;バージョンを使用することをお勧めします。

次の一連の手順は、UNIXベースのコマンド・ライン・ターミナルを使用して行われますが、Windowsターミナルを使用する場合と同じである必要があります。

1. コマンドラインターミナルを開き、Maven がインストールされ、コマンドラインのパスに追加されていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 次のコマンドを実行して、**adobe-public**&#x200B;プロファイルがアクティブであることを確認します。

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

   **参照しない**&#x200B;場合は、**adobe-public**&#x200B;を参照してください。これは、`~/.m2/settings.xml`ファイルでAdobeレポートが正しく参照されていないことを示しています。 [ローカル開発環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)にApache Mavenをインストールして設定する手順を再度実行してください。

1. AEMプロジェクトを生成するディレクトリに移動します。 これは、プロジェクトのソースコードを管理する任意のディレクトリにすることができます。 例えば、ユーザーのホームディレクトリの下に`code`という名前のディレクトリがあるとします。

   ```shell
   $ cd ~/code
   ```

1. 次をコマンドラインに貼り付けて、[バッチモード](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)でプロジェクトを生成します。

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
   >デフォルトでは、Mavenアーキタイプからプロジェクトを生成する際に、インタラクティブモードが使用されます。 バッチモードを使用して生成した値を、太字運転を避けるため。 [AEM Developer Tools plugin for Eclipse](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/aem-eclipse.html)を使用して、Maven AEMプロジェクトを作成することもできます。

   >[!CAUTION]
   >
   >次のようなエラーが表示される場合：*プロジェクトstandalone-pomで、goal org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate(default-cli)を実行できませんでした：目的のアーキタイプが存在しません*。 これは、`~/.m2/settings.xml`ファイル内でAdobeレポートが正しく参照されていないことを示しています。 前の手順を再度実行し、settings.xmlファイルがAdobeレポートを参照していることを確認してください。

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
   | contentFolderName | wnd | /content フォルダー名 |
   | confFolderName | wnd | /conf フォルダー名 |
   | cssId | wnd | 生成された CSS で使用されるプレフィックス |
   | packageGroup | wnd | コンテンツパッケージグループ名 |
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

## プロジェクトを構築{#build}

新しいプロジェクトが生成されたので、プロジェクトコードをAEMのローカルインスタンスにデプロイできます。

1. AEMのインスタンスがローカルでポート&#x200B;**4502**&#x200B;で実行されていることを確認します。
1. コマンドラインから`aem-guides-wknd`プロジェクトディレクトリに移動します。

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

   Mavenプロファイル`autoInstallSinglePackage`は、プロジェクトの個々のモジュールをコンパイルし、1つのパッケージをAEMインスタンスに展開します。 デフォルトでは、このパッケージは、ローカルでポート&#x200B;**4502**&#x200B;を実行し、`admin:admin`の資格情報を持つAEMインスタンスに展開されます。

1. ローカルAEMインスタンスでPackage Managerに移動します。[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). `aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.content`、`aem-guides-wknd.all`の3つのパッケージが見えます。

   また、[AEM Core Components](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)の複数のパッケージが、アーキタイプによってプロジェクトに含まれていることも確認できます。 これについては、チュートリアルで後述します。

1. サイトコンソールに移動します。[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). サイトの 1 つに WKND サイトがあります。このレポートには、米国と言語のマスターの階層を持つサイト構造が含まれます。 このサイト階層は、アーキタイプを使用してプロジェクトを生成する際の`language_country`と`isSingleCountryWebsite`の値に基づいています。

1. ページを選択し、メニューバーの「**編集**」ボタンをクリックして、&lt;a0/>米国&#x200B;**`>`**&#x200B;英語&#x200B;**ページを開きます。**

   ![サイトコンソール](assets/project-setup/aem-sites-console.png)

1. いくつかの作成済みコンテンツおよびページに追加できるコンポーネントがあります。これらのコンポーネントを使用してみることで、機能について大まかに把握できます。このページおよびコンポーネントがどのように設定されているかについては、このチュートリアルの後半で詳細に説明します。

## プロジェクト{#project-structure}をInspect

AEMアーキタイプは、個々のMavenモジュールで構成されています。

* [コア](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - OSGiサービス、リスナーまたはスケジューラーなどのすべてのコア機能に加え、サーブレットや要求フィルターなどのコンポーネント関連のJavaコードを含むJavaバンドル。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  — プロジェクトの/apps部分、ie JS&amp;CSSクライアントライブラリ、コンポーネント、OSGi設定を含みます。
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 編集可能なテンプレート、メタデータスキーマ(/content、/conf)などの構造コンテンツと設定を含みます。
* ui.tests - サーバー側で実行される JUnit テストが含まれる Java バンドル。このバンドルは実稼動環境にはデプロイされません。
* ui.launcher - ui.tests バンドル（および依存するバンドル）をサーバーにデプロイし、JUnit のリモート実行をトリガーするグルーコードが含まれます。
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — （オプション）は、Webpackベースのフロントエンドビルドモジュールを使用するのに必要なアーティファクトを含みます。
* all — これは空のMavenモジュールで、上記のモジュールを1つのパッケージに組み合わせてAEM環境に展開できます。

![Mavenプロジェクト図](assets/project-setup/project-pom-structure.png)

Mavenモジュールの詳細については、[AEM Project Archetypeドキュメント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)を参照してください。

## 高度なMavenコマンド{#advanced-maven-commands}

開発中は、モジュールの1つだけで作業を行う場合があり、時間を節約するためにプロジェクト全体を構築しないようにしたいと考えています。 また、AEM発行インスタンスに直接デプロイしたり、ポート4502で実行されていないAEMのインスタンスにデプロイしたりすることもできます。

次に、開発時の柔軟性を高めるために使用できる追加のMavenプロファイルとコマンドをいくつか見てみましょう。

### コアモジュール{#core-module}

**[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)**&#x200B;モジュールには、プロジェクトに関連付けられたすべてのJavaコードが含まれています。 構築時にOSGiバンドルをAEMにデプロイします。 このモジュールだけを構築するには：

1. `core`フォルダー（`aem-guides-wknd`の下）に移動します。

   ```shell
   $ cd core/
   ```

1. 次の コマンドを実行します。

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

1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)に移動します。 これはOSGi Webコンソールで、AEMインスタンスにインストールされているすべてのバンドルに関する情報が含まれています。

1. **ID**&#x200B;ソート列を切り替えると、WKNDバンドルがインストールされ、アクティブになっていることがわかります。

   ![コアバンドル](assets/project-setup/wknd-osgi-console.png)

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar)には、jarの「物理」位置が表示されます。

   ![CRXDE Location of Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.appsとUi.contentモジュール{#apps-content-module}

**[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** mavenモジュールには、`/apps`の下のサイトに必要なすべてのレンダリングコードが含まれています。 これには CSS／JS が含まれ、それらは [clientlibs](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html) と呼ばれる AEM の形式で保存されます。また、これには動的 HTML をレンダリングするための [HTL](https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html) スクリプトも含まれます。**ui.apps**&#x200B;モジュールは、JCRの構造へのマップと考えることができますが、ファイルシステムに保存してソース管理にコミットすることができます。 **ui.apps**&#x200B;モジュールにはコードのみが含まれています。

このモジュールを構築するには：

1. コマンドラインから `ui.apps`フォルダー（`aem-guides-wknd`の下）に移動します。

   ```shell
   $ cd ../ui.apps
   ```

1. 次の コマンドを実行します。

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

1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)に移動します。 `ui.apps`パッケージは最初にインストールされたパッケージとして表示され、他のパッケージよりも新しいタイムスタンプが必要です。

   ![Ui.appsパッケージがインストールされました](assets/project-setup/ui-apps-package.png)

1. コマンドラインに戻り、次のコマンドを実行します（`ui.apps`フォルダー内）。

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

   プロファイル`autoInstallPackagePublish`は、ポート&#x200B;**4503**&#x200B;で実行されている発行環境にパッケージを展開することを目的としています。 http://localhost:4503で実行中のAEMインスタンスが見つからない場合、上記のエラーが発生します。

1. 最後に、次のコマンドを実行して`ui.apps`パッケージをポート&#x200B;**4504**&#x200B;に展開します。

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

   ポート&#x200B;**4504**&#x200B;で実行中のAEMインスタンスが使用できない場合は、ビルドエラーが発生することが予測されます。 パラメーター`aem.port`は、`aem-guides-wknd/pom.xml`にあるPOMファイルで定義されています。

**[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)**&#x200B;モジュールは、**ui.apps**&#x200B;モジュールと同じ構造になっています。 唯一の違いは、**ui.content**&#x200B;モジュールには&#x200B;**mutable**&#x200B;と呼ばれる内容が含まれている点です。 **Mutablecontentは、** 基本的に、AEMインスタンス上で直接変更できるソース管理 **** ボタンに格納された、テンプレート、ポリシー、フォルダー構造などの非コード設定を指します。これについては、「ページとテンプレート」の章で詳しく説明します。 現時点で重要な点は、**ui.apps**&#x200B;モジュールの構築に使用するのと同じMavenコマンドを&#x200B;**ui.content**&#x200B;モジュールの構築に使用できる点です。 上記の手順は、**ui.content**&#x200B;フォルダー内から自由に繰り返してください。

### Ui.frontend module {#ui-frontend-module}

**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;モジュールはMavenモジュールで、実際には[webpack](https://webpack.js.org/)プロジェクトです。 このモジュールは、JavaScriptファイルとCSSファイルを出力し、AEMにデプロイする専用のフロントエンドビルドシステムとして設定されています。 **ui.frontend**&#x200B;モジュールを使用すると、開発者は[Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/)のような言語でコードを記述し、[npm](https://www.npmjs.com/)モジュールを使用して出力を直接AEMに統合できます。

**ui.frontend**&#x200B;モジュールの詳細は、クライアント側のライブラリとフロントエンドの開発に関する章で説明します。 ここでは、プロジェクトにどのように統合されているかを見てみましょう。

1. コマンドラインから `ui.frontend`フォルダー（`aem-guides-wknd`の下）に移動します。

   ```shell
   $ cd ../ui.frontend
   ```

1. 次の コマンドを実行します。

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

   `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`のような行に注目してください。 これは、コンパイルされたCSSとJSが`ui.apps`フォルダーにコピーされることを示します。

1. ファイル`aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`に対して変更されたタイムスタンプの表示。 `ui.apps`モジュール内の他のファイルよりも新しく更新する必要があります。

   他のモジュールとは異なり、**ui.frontend**&#x200B;モジュールはAEMに直接デプロイするわけではありません。 代わりに、CSSとJSが&#x200B;**ui.apps**&#x200B;モジュールにコピーされ、**ui.apps**&#x200B;モジュールがAEMにデプロイされます。 最初のMavenコマンドのビルド順を見ると、**ui.frontend**&#x200B;は常に&#x200B;** **ui.apps**&#x200B;の前にビルドされます。

   このチュートリアルの後半では、**ui.frontend**&#x200B;モジュールと組み込みのwebpack開発サーバの高度な機能を見て、開発を迅速に行います。

## コアコンポーネントを組み込む {#core-components}

アーキタイプは、プロジェクトに[AEMコアコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)を自動的に埋め込みます。 以前は、デプロイしたパッケージをAEMに対して検査する場合、コアコンポーネントに関連する複数のパッケージが含まれていました。 コアコンポーネントは、AEM Sites プロジェクトの開発を迅速化するために設計された一連の基本コンポーネントです。コアコンポーネントはオープンソースであり、[GitHub](https://github.com/adobe/aem-core-wcm-components) で利用できます。プロジェクトに含まれるコアコンポーネントの詳細については、[を参照してください。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)

1. お気に入りのテキストエディタを使って`aem-guides-wknd/pom.xml`を開きます。

1. `core.wcm.components.version` を検索。これは、どのバージョンのコアコンポーネントが含まれているかを示します。

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEMプロジェクトのアーキタイプにはAEMコアコンポーネントのバージョンが含まれますが、これらのプロジェクトのリリースサイクルは異なるので、含まれるコアコンポーネントのバージョンは最新ではない場合があります。 ベストプラクティスとして、常に最新バージョンのコアコンポーネントを利用するように検討する必要があります。 新機能とバグ修正は頻繁に更新されます。 最新の[リリース情報は、GitHub](https://github.com/adobe/aem-core-wcm-components/releases)にあります。

1. `dependencies`セクションまで下にスクロールすると、個々のコアコンポーネントの依存関係が表示されます。

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

ui.apps の下に多数の .content.xml ファイルが作成されています。これらの XML ファイルは、JCR にインストールされているコンテンツのノードタイプおよびプロパティをマッピングします。これらのファイルは重要であり、****&#x200B;は無視しないでください。

AEMプロジェクトのアーキタイプは、ファイルを安全に無視できる開始点として使用できるサンプル`.gitignore`ファイルを生成します。 ファイルは`<src>/aem-guides-wknd/.gitignore`に生成されます。

## レビュー {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## バリデーターが{#congratulations}

初めてのAEMプロジェクトが作成されました。

### 次の手順 {#next-steps}

[基本コンポーネント](component-basics.md)チュートリアルを使用した簡単な`HelloWorld`例を通して、Adobe Experience Manager(AEM)サイトコンポーネントの基盤となる技術を理解します。
