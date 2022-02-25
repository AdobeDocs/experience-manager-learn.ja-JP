---
title: AEM Sitesの概要 — プロジェクトの設定
seo-title: Getting Started with AEM Sites - Project Setup
description: AEM Sites のコードおよび設定を管理するための、Maven のマルチモジュールプロジェクトの作成について説明します。
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1818'
ht-degree: 13%

---

# プロジェクトのセットアップ {#project-setup}

このチュートリアルでは、Adobe Experience Manager Site のコードと設定を管理するための Maven マルチモジュールプロジェクトの作成について説明します。

## 前提条件 {#prerequisites}

設定に必要なツールと手順を確認します。 [ローカル開発環境](./overview.md#local-dev-environment). Adobe Experience Managerの新しいインスタンスがローカルで使用でき、追加のサンプル/デモパッケージ（必要なサービスパック以外）がインストールされていないことを確認します。

## 目的 {#objective}

1. Maven アーキタイプを使用して新しいAEMプロジェクトを生成する方法を説明します。
1. AEMプロジェクトアーキタイプで生成される様々なモジュールとそれらの連携の仕組みを理解します。
1. AEMコアコンポーネントがAEMプロジェクトにどのように含まれるかを説明します。

## 作成する内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

この章では、 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). AEMプロジェクトには、Sites 実装で使用されるすべてのコード、コンテンツ、設定が含まれています。 この章で生成されるプロジェクトは、WKND サイトの実装の基礎となり、今後の章で構築される予定です。

**Maven プロジェクトとは** - [Apache Maven](https://maven.apache.org/) は、プロジェクトを構築するためのソフトウェア管理ツールです。 *すべてのAdobe Experience Manager* 実装では、Maven プロジェクトを使用して、AEM上にカスタムコードを作成、管理およびデプロイします。

**Maven アーキタイプとは** - A [Maven アーキタイプ](https://maven.apache.org/archetype/index.html) は、新しいプロジェクトを生成するためのテンプレートまたはパターンです。 AEMプロジェクトのアーキタイプを使用すると、カスタム名前空間を持つ新しいプロジェクトを生成し、ベストプラクティスに従ったプロジェクト構造を含めて、プロジェクトを大幅に高速化できます。

## プロジェクトを作成する {#create}

AEM用の Maven マルチモジュールプロジェクトを作成する方法はいくつかあります。 このチュートリアルでは、 [Maven AEM Project Archetype **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager も [UI ウィザードを提供します。](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html) :AEMアプリケーションプロジェクトの作成を開始します。 Cloud Manager UI で生成される基になるプロジェクトの結果は、アーキタイプを直接使用する場合と同じ構造になります。

>[!NOTE]
>
>このチュートリアルではバージョンを使用します **35** を使用します。 常に、 **latest** 新しいプロジェクトを生成するためのアーキタイプのバージョン。

次の一連の手順は、UNIX ベースのコマンドラインターミナルを使用して実行されますが、Windows ターミナルを使用する場合は同様の手順を実行する必要があります。

1. コマンドラインターミナルを開きます。 Maven がインストールされていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. AEMプロジェクトを生成するディレクトリに移動します。 これには、プロジェクトのソースコードを管理する任意のディレクトリを指定できます。 例えば、 `code` ユーザーのホームディレクトリの下に、次の設定を行います。

   ```shell
   $ cd ~/code
   ```

1. 次の内容をコマンドラインに貼り付けます。 [プロジェクトをバッチモードで生成](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=35 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.10 以降をターゲットにする場合は、 `aemVersion="cloud"` と `aemVersion="6.5.10"`.

   プロジェクトを設定するために使用できるプロパティの完全なリスト [ここにあります](https://github.com/adobe/aem-project-archetype#available-properties).

1. 次のフォルダーおよびファイル構造は、ローカルファイルシステム上の Maven アーキタイプによって生成されます。

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## プロジェクトのデプロイとビルド {#build}

プロジェクトコードをビルドし、AEMのローカルインスタンスにデプロイします。

1. AEMのオーサーインスタンスがポート上でローカルで実行されていることを確認します。 **4502**.
1. コマンドラインから、 `aem-guides-wknd` プロジェクトディレクトリ。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 次のコマンドを実行して、プロジェクト全体を構築し、AEMにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ビルドは約 1 分かかり、最後に次のメッセージが表示されます。

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven プロファイル `autoInstallSinglePackage` プロジェクトの個々のモジュールをコンパイルし、1 つのパッケージをAEMインスタンスにデプロイします。 デフォルトでは、このパッケージは、ポートでローカルに実行されているAEMインスタンスにデプロイされます **4502** そして `admin:admin`.

1. ローカルのAEMインスタンスでパッケージマネージャーに移動します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). のパッケージが表示されます。 `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`、および `aem-guides-wknd.all`.

1. サイトコンソールに移動します。 [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). サイトの 1 つに WKND サイトがあります。US と言語の階層を持つサイト構造が含まれますマスター。 このサイト階層は、 `language_country` および `isSingleCountryWebsite` アーキタイプを使用してプロジェクトを生成する際に使用します。

1. を開きます。 **US** `>` **英語** ページを表示するには、ページを選択して **編集** ボタンをクリックします。

   ![サイトコンソール](assets/project-setup/aem-sites-console.png)

1. スターターコンテンツは既に作成されており、ページに追加できる複数のコンポーネントが用意されています。 これらのコンポーネントを使用してみることで、機能について大まかに把握できます。次の章では、コンポーネントの基本について学びます。

   ![ホームスターターコンテンツ](assets/project-setup/start-home-page.png)

   *アーキタイプで生成されたサンプルコンテンツ*

## Inspect {#project-structure}

生成されたAEMプロジェクトは、個々の Maven モジュールで構成され、それぞれ異なる役割を持ちます。 このチュートリアルと大部分の開発では、次のモジュールに焦点を当てています。

* [コア](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java コード。主にバックエンド開発者。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja)  — 主にフロントエンド開発者向けの CSS、JavaScript、Sass、Type Script のソースコードが含まれます。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — コンポーネントおよびダイアログ定義が含まれ、コンパイル済みの CSS と JavaScript がクライアントライブラリとして埋め込まれます。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 編集可能なテンプレート、メタデータスキーマ (/content、/conf) などの構造的なコンテンツや設定が含まれます。

* **すべて**  — これは、上記のモジュールを組み合わせた空の Maven モジュールで、AEM環境にデプロイできる単一のパッケージになります。

![Maven プロジェクト図](assets/project-setup/project-pom-structure.png)

詳しくは、 [AEM Project Archetype ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 詳細を学ぶ **すべて** Maven モジュール。

### コアコンポーネントを組み込む {#core-components}

[AEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja) は、AEM用の標準化された Web コンテンツ管理 (WCM) コンポーネントのセットです。 これらのコンポーネントは、機能のベースラインセットを提供し、個々のプロジェクトに対してスタイル設定、カスタマイズ、拡張を行うように設計されています。

AEMas a Cloud Service環境には、 [AEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). したがって、AEMas a Cloud Service用に生成されたプロジェクトでは、 **not** AEMコアコンポーネントの埋め込みを含めます。

AEM 6.5/6.4 で生成されたプロジェクトの場合、アーキタイプは自動的に埋め込みます [AEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) プロジェクト内で使用されます。 AEM 6.5/6.4 では、AEMコアコンポーネントを埋め込んで、最新バージョンがプロジェクトに確実にデプロイされるようにすることがベストプラクティスです。 コアコンポーネントの機能について詳しくは、 [プロジェクトに含まれるは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## ソース管理システムによる管理 {#source-control}

アプリケーションのコードを管理するために、何らかのソース管理システムを使用することが常に推奨されます。このチュートリアルでは git および GitHub を使用します。Maven などの任意の IDE では、SCM で無視すべきいくつかのファイルが生成されます。

Maven は、コードパッケージをビルドおよびインストールするたびにターゲットフォルダーを作成します。ターゲットフォルダーとコンテンツは、SCM から除外する必要があります。

の下 `ui.apps` 多くの人々を観察する `.content.xml` ファイルが作成されます。 これらの XML ファイルは、JCR にインストールされているコンテンツのノードタイプおよびプロパティをマッピングします。これらのファイルは重要で、 **not** 無視されます。

AEMプロジェクトアーキタイプでは、サンプルが生成されます `.gitignore` ファイルを安全に無視できる出発点として使用できるファイル。 ファイルは次の場所に生成されます。 `<src>/aem-guides-wknd/.gitignore`.

## おめでとうございます。 {#congratulations}

これで、最初のAEMプロジェクトが作成されました。

### 次の手順 {#next-steps}

シンプルなを使用して、Adobe Experience Manager(AEM)Sites コンポーネントの基盤となるテクノロジーを理解します `HelloWorld` 例 [コンポーネントの基本](component-basics.md) チュートリアル

## 高度な Maven コマンド（ボーナス） {#advanced-maven-commands}

開発時には、1 つのモジュールだけを使用して作業している可能性があり、時間を節約するために、プロジェクト全体を構築したくない場合があります。 また、AEM パブリッシュインスタンスに直接デプロイする場合や、ポート 4502 で実行されていないAEMのインスタンスにデプロイする場合もあります。

次に、開発時の柔軟性を高めるために使用できる、追加の Maven プロファイルおよびコマンドについて説明します。

### コアモジュール {#core-module}

この **[コア](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** モジュールには、プロジェクトに関連付けられたすべての Java コードが含まれます。 ビルド時に、AEMに OSGi バンドルがデプロイされます。 このモジュールのみを構築するには：

1. 次に移動： `core` フォルダー ( `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. に移動します。 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). これは OSGi Web コンソールで、AEMインスタンスにインストールされているすべてのバンドルに関する情報が含まれます。

1. 切り替え **ID** 並べ替え列が表示され、WKND バンドルがインストールされ、アクティブになっていることが確認できます。

   ![コアバンドル](assets/project-setup/wknd-osgi-console.png)

1. jar の「物理的な」場所は、 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![JAR の CRXDE の場所](assets/project-setup/jcr-bundle-location.png)

### Ui.apps および Ui.content モジュール {#apps-content-module}

この **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven モジュールには、の下のサイトで必要となるすべてのレンダリングコードが含まれます。 `/apps`. これには CSS／JS が含まれ、それらは [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja) と呼ばれる AEM の形式で保存されます。また、これには動的 HTML をレンダリングするための [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=ja) スクリプトも含まれます。以下の点について考えてみてください。 **ui.apps** モジュールは、JCR 内の構造に対するマップとして、ファイルシステムに保存でき、ソース管理にコミットできる形式です。 この **ui.apps** モジュールにはコードのみが含まれています。

このモジュールのみを構築するには：

1. コマンドラインから。 次に移動： `ui.apps` フォルダー ( `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. に移動します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 次のように表示されます。 `ui.apps` パッケージは最初にインストールされたパッケージとして使用し、他のパッケージのタイムスタンプよりも新しいタイムスタンプを使用する必要があります。

   ![Ui.apps パッケージがインストールされています](assets/project-setup/ui-apps-package.png)

1. コマンドラインに戻り、次のコマンドを実行します ( `ui.apps` フォルダー ):

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

   プロファイル `autoInstallPackagePublish` は、ポートで実行されているパブリッシュ環境にパッケージをデプロイすることを目的としています **4503**. 上記のエラーは、http://localhost:4503で実行中のAEMインスタンスが見つからない場合に発生します。

1. 最後に、次のコマンドを実行して、 `ui.apps` ポート上のパッケージ **4504**:

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

   この場合も、ポートでAEMインスタンスが実行されていない場合、ビルドエラーが発生することが予想されます **4504** が使用可能です。 パラメーター `aem.port` は、次の場所にある POM ファイルで定義されます。 `aem-guides-wknd/pom.xml`.

この **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** モジュールは、 **ui.apps** モジュール。 唯一の違いは **ui.content** モジュールには、 **可変** コンテンツ。 **可変** コンテンツとは、基本的に、ソース管理下に保存されている、テンプレート、ポリシー、フォルダー構造などの非コード設定を指します。 **しかし** をAEMインスタンス上で直接変更できます。 これについては、ページとテンプレートの章で詳しく説明します。

をビルドするために使用するのと同じ Maven コマンド **ui.apps** モジュールは、 **ui.content** モジュール。 上記の手順を、 **ui.content** フォルダー。

## トラブルシューティング

AEMプロジェクトアーキタイプを使用したプロジェクトの生成で問題が発生した場合は、 [既知の問題](https://github.com/adobe/aem-project-archetype#known-issues) およびオープンのリスト [問題](https://github.com/adobe/aem-project-archetype/issues).
