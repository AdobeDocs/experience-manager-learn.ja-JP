---
title: AEM Sites の基本を学ぶ - プロジェクトの設定
description: Maven マルチモジュールプロジェクトを作成して、Experience Manager サイトのコードと設定を管理します。
version: 6.5, Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 578
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1684'
ht-degree: 100%

---

# プロジェクトのセットアップ {#project-setup}

このチュートリアルでは、Adobe Experience Manager サイトのコードと設定を管理するための Maven Multi Module Project の作成について説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](./overview.md#local-dev-environment)のセットアップに必要なツールと手順を確認します。Adobe Experience Manager の新しいインスタンスがローカルで使用でき、必要なサービスパック以外、その他のサンプル/デモパッケージがインストールされていないことを確認します。

## 目的 {#objective}

1. Maven アーキタイプを使用して新しい AEM プロジェクトを生成する方法を説明します。
1. AEM プロジェクトアーキタイプで生成される様々なモジュールと、それらが連携する仕組みを理解します。
1. AEM コアコンポーネントが AEM プロジェクトにどのように含まれるかを理解します。

## 作ろうとしているもの {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

この章では、[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用して、新しい Adobe Experience Manager プロジェクトを生成します。AEM プロジェクトには、Sites の実装に使用する完全なコード、コンテンツ、設定が含まれています。この章で生成されるプロジェクトは、WKND サイトの実装の基礎となり、今後の章で構築されます。

**Maven プロジェクトについて** - [Apache Maven](https://maven.apache.org/) は、プロジェクトを構築するためのソフトウェア管理ツールです。*すべての Adobe Experience Manager* の実装では、Maven プロジェクトを使用して、AEM 上にカスタムコードを作成、管理およびデプロイします。

**Maven アーキタイプについて** - [Maven アーキタイプ](https://maven.apache.org/archetype/index.html)は、新しいプロジェクトを生成するためのテンプレートまたはパターンです。AEM プロジェクトアーキタイプを使用すると、カスタム名前空間で新しいプロジェクトを生成し、ベストプラクティスに従ったプロジェクト構造を含め、プロジェクトの開発を大幅に高速化することができます。

## プロジェクトの作成 {#create}

AEM 用の Maven マルチモジュールプロジェクトを作成するには、いくつかのオプションがあります。このチュートリアルでは、[Maven AEM プロジェクトアーキタイプ **35**](https://github.com/adobe/aem-project-archetype) を使用します。Cloud Manager は、AEM アプリケーションプロジェクトの作成を開始するための [UI ウィザード](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=ja)も提供します。Cloud Manager UI で生成される基になるプロジェクトの結果は、アーキタイプを直接使用する場合と同じ構造になります。

>[!NOTE]
>
>このチュートリアルでは、アーキタイプのバージョン **35** を使用します。**最新の**&#x200B;バージョンのアーキタイプを使用して新しいプロジェクトを生成することは、常にベストプラクティスです。

次の一連の手順は、UNIX® ベースのコマンドラインターミナルを使用して行いますが、Windows ターミナルを使用する場合も同様です。

1. コマンドラインターミナルを開きます。Maven がインストールされていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. AEM プロジェクトを生成するディレクトリに移動します。これには、プロジェクトのソースコードを管理する任意のディレクトリを指定できます。例えば、ユーザーのホームディレクトリの下にある `code` という名前のディレクトリで次の操作を行います。

   ```shell
   $ cd ~/code
   ```

1. 以下をコマンドラインに貼り付けて、[バッチモードでプロジェクトを生成](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)します。

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
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
   > AEM 6.5.14+ をターゲットするには、`aemVersion="cloud"` を `aemVersion="6.5.14"` に置き換えます。
   >
   > また、[AEM プロジェクトアーキタイプ／使用状況](https://github.com/adobe/aem-project-archetype#usage)を参照して常に最新の `archetypeVersion` を使用します。

   プロジェクトの設定に使用できるプロパティの完全なリストは、[こちら](https://github.com/adobe/aem-project-archetype#avilable-properties)にあります。

1. 次のフォルダーおよびファイル構造が、ローカルファイルシステム上の Maven アーキタイプによって生成されます。

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
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## プロジェクトをデプロイしてビルドします。 {#build}

プロジェクトコードをビルドして、AEM のローカルインスタンスにデプロイします。

1. ポート **4502** でローカルに実行されている AEM のオーサーインスタンスがあることを確認します。
1. コマンドラインから `aem-guides-wknd` プロジェクトディレクトリに移動します。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 以下のコマンドを実行してプロジェクト全体をビルドし、AEM にデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ビルドは 1 分程度かかり、最後に次のメッセージが表示されます。

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Maven プロファイル `autoInstallSinglePackage` は、プロジェクトの個々のモジュールをコンパイルし、1 つのパッケージを AEM インスタンスにデプロイします。デフォルトでは、このパッケージは、ポート **4502** でローカルに実行され、資格情報が `admin:admin` である AEM インスタンスにデプロイされます。

1. ローカル AEM インスタンス上で、パッケージマネージャーに移動します（[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)）。`aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.config`、`aem-guides-wknd.ui.content`、`aem-guides-wknd.all` のパッケージが表示されます。

1. Sites コンソールに移動します（[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)）。サイトの 1 つに WKND サイトがあります。US と言語のマスターの階層を持つサイト構造が含まれています。このサイト階層は、アーキタイプを使用してプロジェクトを生成するときの `language_country` と `isSingleCountryWebsite` の値に基づいています。

1. **US** `>` **英語**&#x200B;ページを選択し、メニューバーの「**編集**」ボタンをクリックしてページを開きます。

   ![サイトコンソール](assets/project-setup/aem-sites-console.png)

1. スターターコンテンツは既に作成されており、ページに追加できるコンポーネントがいくつかあります。これらのコンポーネントを使用してみることで、機能について大まかに把握できます。次の章では、コンポーネントの基本について説明します。

   ![ホームスターターコンテンツ](assets/project-setup/start-home-page.png)

   *アーキタイプで生成されたサンプルコンテンツ*

## プロジェクトの検査 {#project-structure}

生成された AEM プロジェクトは、個別の Maven モジュールで構成され、それぞれ異なる役割を持ちます。このチュートリアルとほとんどの開発では、次のモジュールに焦点を当てています。

* [コア](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=ja) - Java コードが含まれています。主にバックエンド開発者向け。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja) - CSS、JavaScript、Sass、TypeScript のソースコードが含まれています。主にフロントエンド開発者向け。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=ja) - コンポーネントおよびダイアログ定義が含まれ、コンパイル済みの CSS と JavaScript がクライアントライブラリとして埋め込まれています。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=ja) - 編集可能なテンプレート、メタデータスキーマ（/content、/conf）などの構造的なコンテンツや設定が含まれています。

* **all** - 空の Maven モジュールで、上記のモジュールを 1 つのパッケージにまとめ、AEM 環境にデプロイできるようにしたものです。

![Maven プロジェクト図](assets/project-setup/project-pom-structure.png)

**すべての** Maven モジュールについて詳しくは、[AEM プロジェクトアーキタイプのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)を参照してください。

### コアコンポーネントを組み込む {#core-components}

[AEM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)は、AEM 用の標準化された一連の web コンテンツ管理（WCM）コンポーネントです。これらのコンポーネントは、機能のベースラインのセットを提供し、個々のプロジェクトに対してスタイル設定、カスタマイズ、拡張を行います。

AEM as a Cloud Service 環境には、最新バージョンの [AEM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja) が含まれています。したがって、AEM as a Cloud Service 用に生成されたプロジェクトには、AEM コアコンポーネントの埋め込みが&#x200B;**含まれません**。

AEM 6.5 または 6.4 で生成されたプロジェクトの場合、アーキタイプは [AEM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)をプロジェクトに自動的に埋め込みます。AEM 6.5 または 6.4 では、AEM コアコンポーネントを埋め込んで、最新バージョンがプロジェクトに確実にデプロイされるようにすることがベストプラクティスです。コアコンポーネントがプロジェクトにどのように含まれているかについての詳細は、[こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=ja#core-components)を参照してください。

## ソース制御管理 {#source-control}

アプリケーション内のコードを管理するには、何らかの形のソース制御を使用することをお勧めします。このチュートリアルでは Git と GitHub を使用します。Maven や選択した IDE で生成されるファイルの中には、SCM で無視されるべきものがいくつかあります。

Maven は、コードパッケージをビルドおよびインストールするたびにターゲットフォルダーを作成します。ターゲットフォルダーおよびコンテンツは SCM から除外する必要があります。

`ui.apps` モジュールでは、`.content.xml` ファイルが多数作成されることを確認します。これらの XML ファイルは、JCR にインストールされているコンテンツのノードタイプおよびプロパティをマッピングします。これらのファイルは重要であり、**無視することはできません**。

AEM プロジェクトアーキタイプはサンプルの `.gitignore` ファイルを生成します。まずはこのファイルを使用して、安全に無視できるファイルを判断します。ファイルは `<src>/aem-guides-wknd/.gitignore` に生成されます。

## おめでとうございます。 {#congratulations}

これで、最初の AEM プロジェクトが作成されました。

### 次の手順 {#next-steps}

[コンポーネントの基本](component-basics.md)チュートリアルのシンプルな `HelloWorld` 例を通して、Adobe Experience Manager (AEM) Sites コンポーネントの基礎となるテクノロジーを理解します。

## 高度な Maven コマンド（ボーナス） {#advanced-maven-commands}

開発時には、1 つのモジュールだけを使用して作業を行う場合があり、時間を節約するために、プロジェクト全体を構築したくない場合があります。また、AEM パブリッシュインスタンスに直接デプロイする場合や、ポート 4502 で実行されていない AEM のインスタンスにデプロイする場合もあります。

次に、開発中の柔軟性を高めるために使用できる Maven のその他のプロファイルおよびコマンドをいくつか見てみましょう。

### コアモジュール {#core-module}

この&#x200B;**[コア](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=ja)**&#x200B;モジュールには、プロジェクトに関連付けられたすべての Java™コードが含まれます。**コア**&#x200B;モジュールのビルドは、AEM に OSGi バンドルをデプロイします。このモジュールのみをビルドするには：

1. `core` フォルダー（`aem-guides-wknd` の下）に移動します。

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

1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) に移動します。これは OSGi web コンソールで、AEM インスタンスにインストールされているすべてのバンドルに関する情報が含まれます。

1. **ID** の並べ替え列を切り替えると、WKND バンドルがインストールされ、アクティブになっていることが確認できます。

   ![コアバンドル](assets/project-setup/wknd-osgi-console.png)

1. JAR の「物理的」な場所は [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) で確認できます。

   ![JAR の CRXDE の場所](assets/project-setup/jcr-bundle-location.png)

### Ui.apps および Ui.content モジュール {#apps-content-module}

**[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=ja)** maven モジュールには、`/apps` 配下のサイトで必要となるすべてのレンダリングコードが含まれます。これには CSS/JS が含まれ、[clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja) と呼ばれる AEM の形式で保存されます。また、これには動的 HTML をレンダリングするための [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=ja) スクリプトも含まれます。**ui.apps** モジュールは、ファイルシステムに保存でき、ソースコントロールに対してコミットできる、JCR 内の構造へのマップと考えることができます。**ui.apps** モジュールにはコードのみが含まれています。

このモジュールのみをビルドするには：

1. コマンドラインから。`ui.apps` フォルダー（`aem-guides-wknd` の下）に移動します。

   ```shell
   $ cd ../ui.apps
   ```

1. 次のコマンドを実行します。

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp) に移動します。 `ui.apps` パッケージが、最初にインストールされたパッケージとして表示され、他のパッケージよりもタイムスタンプが新しいものになっているはずです。

   ![インストールされた Ui.apps パッケージ](assets/project-setup/ui-apps-package.png)

1. コマンドラインに戻り、次のコマンドを実行します（`ui.apps` フォルダー内）。

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   プロファイル `autoInstallPackagePublish` は、ポート **4503** で実行されているパブリッシュ環境にパッケージをデプロイすることを目的としています。上記のエラーは、http://localhost:4503で実行中の AEM インスタンスが見つからない場合に発生します。

1. 最後に、次のコマンドを実行して、**4504** ポート上の `ui.apps` パッケージをデプロイします。

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

   この場合も、ポート **4504** で実行されている AEM インスタンスが利用できない場合、ビルドエラーが発生することが予想されます。 パラメーター `aem.port` は、`aem-guides-wknd/pom.xml` にある POM ファイルで定義されます。

**[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=ja)** モジュールは、**ui.apps** モジュールと同じ方法で構造化されます。 唯一の違いは、**ui.content** モジュールに&#x200B;**可変**&#x200B;コンテンツが含まれることです。 **可変**&#x200B;コンテンツとは基本的に、ソース管理下に保存されている、テンプレート、ポリシー、フォルダー構造などのコード以外の設定を指します。 **ただし**、AEMインスタンス上で直接変更できます。 これについて詳しくは、ページとテンプレートの章で説明しています。

**ui.apps** モジュールをビルドするために使用するのと同じ Maven コマンドは、**ui.content** モジュールのビルドに使用できます。上記の手順を、**ui.content** フォルダー内から自由に繰り返してください。

## トラブルシューティング

AEM プロジェクトアーキタイプを使用したプロジェクトの生成で問題が発生した場合は、[既知の問題](https://github.com/adobe/aem-project-archetype#known-issues)およびオープンの[問題](https://github.com/adobe/aem-project-archetype/issues)リストを確認してください。

## お疲れさまでした。 {#congratulations-bonus}

ボーナスマテリアルを完了しました。

### 次の手順 {#next-steps-bonus}

シンプルな `HelloWorld` の例と[コンポーネントの基本](component-basics.md)チュートリアルを使って、Adobe Experience Manager（AEM）Sites コンポーネントの基盤となるテクノロジーについて理解します。
