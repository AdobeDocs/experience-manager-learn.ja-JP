---
title: ローカル AEM 開発環境の設定
description: Adobe Experience Manager、AEM用のローカル開発の設定ガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングの重要なトピックについて説明します。 Eclipse IDE、CRXDE-Lite、Visual Studio Code、IntelliJを使用した開発について説明します。
version: 6.4, 6.5
feature: 開発者ツール
topics: development
activity: develop
audience: developer
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2655'
ht-degree: 8%

---


# ローカル AEM 開発環境の設定

Adobe Experience Manager、AEM用のローカル開発の設定ガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングの重要なトピックについて説明します。 **[!DNL Eclipse IDE]、[!DNL CRXDE Lite]、[!DNL Visual Studio Code]および[!DNL IntelliJ]**&#x200B;を使用した開発について説明します。

## 概要

Adobe Experience ManagerまたはAEM向けにローカル開発を開発する際は、まず、環境を設定する必要があります。 生産性を高め、より良いコードを迅速に書き込むために、品質の高い開発環境の設定に時間をかけます。 AEMローカル開発環境は、次の4つの領域に分割できます。

* ローカルAEMインスタンス
* [!DNL Apache Maven] project
* 統合開発環境(IDE)
* トラブルシューティング

## ローカルのAEMインスタンスのインストール

ローカルのAEMインスタンスを参照する場合、開発者のパーソナルマシン上で動作するAdobe Experience Managerのコピーについて説明します。 ****** すべてのAEM開発は、まず、ローカルのAEMインスタンスに対してコードを記述して実行する必要があります。

AEMを初めて使用する場合は、次の2つの基本的な実行モードをインストールできます。***オーサー***&#x200B;と&#x200B;***パブリッシュ***。 ***作成者*** [実行モード](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)は、デジタルマーケターがコンテンツの作成と管理に使用する環境です。 オーサーインスタンスにコードをデプロイする&#x200B;**ほとんど**&#x200B;を開発する場合。 これにより、新しいページを作成したり、コンポーネントを追加および設定したりできます。 AEM SitesはWYSIWYGオーサリングCMSなので、CSSとJavaScriptのほとんどはオーサリングインスタンスに対してテストできます。

また、ローカルの&#x200B;***パブリッシュ***&#x200B;インスタンスに対する&#x200B;*重要な*&#x200B;テストコードです。 ***パブリッシュ***&#x200B;インスタンスは、Webサイトの訪問者がやり取りするAEM環境です。 ***パブリッシュ***&#x200B;インスタンスは、***オーサー***&#x200B;インスタンスと同じテクノロジースタックですが、設定と権限に関しては大きな違いがあります。 上位の環境に昇格する前に、コードを常に&#x200B;**&#x200B;ローカルの&#x200B;***パブリッシュ***&#x200B;インスタンスに対してテストする必要があります。

### 手順

1. [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)がインストールされていることを確認します。
   * AEM 6.5以降では、[Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3Alast&amp;orderby.sort=&amp;layout=list&amp;p.offset=0&amp;p.limit=14)を推奨します。
   * [AEM 6.5より前のバージョンのAEM用Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 
2. [AEM QuickStart Jarと [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)のコピーを取得します。
3. 次のようなフォルダー構造をコンピューター上に作成します。

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. [!DNL QuickStart] JARの名前を&#x200B;***aem-author-p4502.jar***&#x200B;に変更し、`/author`ディレクトリの下に配置します。 ***[!DNL license.properties]***&#x200B;ファイルを`/author`ディレクトリの下に追加します。
5. [!DNL QuickStart] JARのコピーを作成し、***aem-publish-p4503.jar***&#x200B;に名前を変更して、`/publish`ディレクトリの下に配置します。 ***[!DNL license.properties]***&#x200B;ファイルのコピーを`/publish`ディレクトリの下に追加します。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. ***aem-author-p4502.jar***&#x200B;ファイルをダブルクリックして、**Author**&#x200B;インスタンスをインストールします。 これにより、ローカルコンピューターのポート&#x200B;**4502**&#x200B;で実行されているオーサーインスタンスが起動します。

   ***aem-publish-p4503.jar***&#x200B;ファイルをダブルクリックして、**パブリッシュ**&#x200B;インスタンスをインストールします。 これにより、ローカルコンピューターのポート&#x200B;**4503**&#x200B;で実行されているパブリッシュインスタンスが起動します。

   >[!NOTE]
   >
   >開発マシンのハードウェアによっては、**オーサーインスタンスとパブリッシュ**&#x200B;インスタンスの両方を同時に実行するのが困難な場合があります。 ローカルセットアップで同時に両方を実行する必要が生じることはほとんどありません。

   詳しくは、[AEMインスタンスのデプロイとメンテナンス](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)を参照してください。

## Apache Mavenのインストール

***[!DNL Apache Maven]*** は、Javaベースのプロジェクトのビルドおよびデプロイ手順を管理するツールです。AEMはJavaベースのプラットフォームで、 AEMプロジェクトのコードを管理する標準的な方法です。 [!DNL Maven]***AEM Mavenプロジェクト***&#x200B;または&#x200B;***AEMプロジェクト***&#x200B;のみと言うと、サイトのすべての&#x200B;*カスタム*&#x200B;コードを含むMavenプロジェクトを指します。

すべてのAEMプロジェクトは、**[!DNL AEM Project Archetype]**&#x200B;の最新バージョンを使用して構築する必要があります。[https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). [!DNL AEM Project Archetype]は、サンプルコードとコンテンツを含むAEMプロジェクトのブートストラップを作成します。 [!DNL AEM Project Archetype]には、プロジェクトで使用するように設定された&#x200B;**[!DNL AEM WCM Core Components]**&#x200B;も含まれます。

>[!CAUTION]
>
>新しいプロジェクトを開始する際には、最新バージョンのアーキタイプを使用することをお勧めします。 アーキタイプには複数のバージョンがあり、すべてのバージョンが以前のバージョンのAEMと互換性があるわけではないことに注意してください。

### 手順

1. [Apache Maven](https://maven.apache.org/download.cgi)をダウンロードします。
2. [Apache Maven](https://maven.apache.org/install.html)をインストールし、インストールがコマンドライン`PATH`に追加されていることを確認します。
   * [!DNL macOS] ユーザーは、Homebrewを使用してMavenをインストールで [きます](https://brew.sh/)
3. 新しいコマンドラインターミナルを開き、次のコマンドを実行して、**[!DNL Maven]**&#x200B;がインストールされていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. **[!DNL adobe-public]**&#x200B;プロファイルを[!DNL Maven] [settings.xml](https://maven.apache.org/settings.html)ファイルに追加して、**[!DNL repo.adobe.com]**&#x200B;をMavenのビルドプロセスに自動的に追加します。

5. `settings.xml`という名前のファイルが存在しない場合は、`~/.m2/settings.xml`に作成します。

6. [](https://repo.adobe.com/)での手順に基づいて、**[!DNL adobe-public]**&#x200B;プロファイルを`settings.xml`ファイルに追加します。

   `settings.xml`の例を以下に示します。 *の命名規則と、ユーザーの `settings.xml` ディレクトリの下の配置が重 `.m2` 要です。*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. 次のコマンドを実行して、**adobe-public**&#x200B;プロファイルがアクティブであることを確認します。

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

   **[!DNL adobe-public]**&#x200B;が表示されない場合は、Adobeリポジトリが`~/.m2/settings.xml`ファイルで正しく参照されていないことを示しています。 前の手順を再度実行し、settings.xmlファイルがAdobeリポジトリを参照していることを確認してください。

## 統合開発環境の設定

統合開発環境またはIDEは、テキストエディタ、構文サポート、ビルドツールを組み合わせたアプリケーションです。 実行する開発の種類によっては、IDEが別のIDEよりも優先される場合があります。 IDEに関係なく、ローカルのAEMインスタンスに&#x200B;***プッシュ***&#x200B;コードを定期的にプッシュしてテストできることが重要です。 また、Gitなどのソース管理システムに持続するためには、ローカルのAEMインスタンスからAEMプロジェクトに&#x200B;***プル***&#x200B;設定を行うことも重要です。

ローカルのAEMインスタンスとの統合を示す、対応するビデオを含む、AEM開発で使用される、より一般的なIDEの例を以下に示します。

>[!NOTE]
>
> WKNDプロジェクトは、AEM as a Cloud Serviceで動作するようにデフォルトに更新されました。 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)と[後方互換性を持つように更新されました。 AEM 6.5または6.4を使用している場合は、任意のMavenコマンドに`classic`プロファイルを追加します。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDEを使用する場合は、「Mavenプロファイル」タブで`classic`を確認してください。

![「Mavenプロファイル」タブ](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Mavenプロファイル*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;は、オープンソースで&#x200B;***無料***&#x200B;なので、Java開発用のIDEの中でも一般的なものの1つです。 Adobeには、**[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**&#x200B;というプラグインが用意されています。[!DNL Eclipse]を使用すると、優れたGUIで開発を容易にし、コードをローカルのAEMインスタンスと同期できます。 [!DNL Eclipse] IDEは、[!DNL AEM Developer Tools]によるGUIのサポートのため、AEMを初めて使用する開発者に推奨されます。

#### インストールとセットアップ

1. [!DNL Eclipse] IDEを[!DNL Java EE Developers]用にダウンロードしてインストールします。[https://www.eclipse.org](https://www.eclipse.org/)
1. [!DNL AEM Developer Tools]プラグインをインストールする手順に従います。[https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Mavenプロジェクトのインポート
* 01:24 - Mavenを使用したソースコードのビルドとデプロイ
* 04:33 - AEM開発者ツールを使用したプッシュコードの変更
* 10:55 - AEM Developer Toolを使用したプルコード変更
* 13:12 - Eclipseの統合デバッグツールの使用

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;は、Javaのプロフェッショナル開発用の強力なIDEです。 [!DNL IntelliJ IDEA] は、フレディションと商 ****** [!DNL Community] 業版（有料）の2種類の味で [!DNL Ultimate] す。[!DNL Community]版の[!DNL IntellIJ IDEA]は、より多くのAEM開発に十分ですが、[!DNL Ultimate] [機能セット](https://www.jetbrains.com/idea/download)を拡張します。

#### [!DNL Installation and Setup]

1. [!DNL IntelliJ IDEA]をダウンロードしてインストールします。[https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. [!DNL Repo] （コマンドラインツール）をインストールします。[https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Mavenプロジェクトのインポート
* 05:47 - Mavenを使用したソースコードのビルドとデプロイ
* 08:17 — リポジトリを使用したプッシュの変更
* 14:39 - Repoを使用した変更のプル
* 17:25 - IntelliJ IDEAの統合デバッグツールの使用

### [!DNL Visual Studio Code]

**[Visual Studio ](https://code.visualstudio.com/)** Codeは、強化されたJavaScriptのサポート、およびブラウザーのデバッ ***グサポートを備えたフロントエ*** ンド開発者にとって、すぐにお気に入りのツールになっていま [!DNL Intellisense]す。**[!DNL Visual Studio Code]** は、多くの強力な拡張機能を備えたオープンソースで、無料です。[!DNL Visual Studio Code] は、Adobeツール **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)を使用して、AEMと統合するように設定できます。** また、AEMと統合するためにインストールできる、コミュニティでサポートされる拡張機能もいくつかあります。

[!DNL Visual Studio Code] は、主にCSS/LESSとJavaScriptコードを記述してAEMクライアントライブラリを作成するフロントエンド開発者に最適です。ノード定義（ダイアログ、コンポーネント）はすべて生のXMLで編集する必要があるので、このツールは新しいAEM開発者にとって最適な選択ではない可能性があります。 [!DNL Visual Studio Code]には複数のJava拡張機能が使用できますが、主にJava開発を行う場合は[!DNL Eclipse IDE]または[!DNL IntelliJ]を使用することをお勧めします。

#### 重要なリンク

* [****](https://code.visualstudio.com/Download) **DownloadVisual Studio Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  - JCRコンテンツ用のFTPに似たツール
* **[aemfed](https://aemfed.io/)**  - AEMフロントエンドワークフローの高速化
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  - Community supported* extension for Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Mavenプロジェクトのインポート
* 0時53分 — Mavenを使用したソースコードのビルドとデプロイ
* 04:03 — リポジトリコマンドラインツールを使用したプッシュコードの変更
* 08:29 - Repoコマンドラインツールを使用したプルコードの変更
* 10:40 - AEMFEDツールを使用したプッシュコードの変更
* 14:24 — トラブルシューティング、クライアントライブラリの再構築

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) は、AEMリポジトリのブラウザーベースのビューです。[!DNL CRXDE Lite] はAEMに埋め込まれており、開発者は、ファイルの編集、コンポーネント、ダイアログ、テンプレートの定義など、標準的な開発タスクを実行できます。[!DNL CRXDE Lite] は、完 ****** 全な開発環境ではなく、デバッグツールとして非常に効果的です。[!DNL CRXDE Lite] は、を拡張する場合や、コードベース外の製品コードについて理解する場合に役立ちます。[!DNL CRXDE Lite] は、リポジトリを強力に表示し、権限を効果的にテストおよび管理する方法を提供します。

[!DNL CRXDE Lite] は、常に他のIDEと組み合わせてコードのテストとデバッグを行う必要がありますが、主な開発ツールとしては使用しないでください。構文のサポートが制限され、オートコンプリート機能はなく、ソース管理システムとの統合も制限されます。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## トラブルシューティング

***ヘルプ!*** コードが機能しない！すべての開発と同様に、コードが期待どおりに動作しない場合も（おそらく多く）あります。 AEMは強力なプラットフォームですが、大きな力を持って…は、大きな複雑性をもたらします。 以下に、問題のトラブルシューティングと追跡に関する概要をいくつか示します（ただし、問題が発生する可能性のあるすべてを網羅したリストからは程遠い）。

### コードのデプロイメントの検証

問題が発生した場合の最初の手順は、コードがAEMに正常にデプロイされ、インストールされていることを確認することです。

1. **パッケージ [!UICONTROL マネージャー]** で、コードパッケージがアップロードおよびインストールされていることを確認します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)を参照してください。タイムスタンプを確認して、パッケージが最近インストールされたことを確認します。
1. [!DNL Repo]や[!DNL AEM Developer Tools]などのツールを使用して増分ファイル更新をおこなう場合は、**ファイルがローカルのAEMインスタンスにプッシュされ、ファイルの内容が更新されたことを確認します。[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)[!DNL CRXDE Lite]**
1. **OSGiバンドル内のJavaコードに関** 連する問題が見つかった場合は、バンドルがアップロードされていることを確認します。[!UICONTROL Adobe Experience Manager Webコンソール]を開きます。[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)を検索し、バンドルを検索します。 バンドルのステータスが&#x200B;**[!UICONTROL アクティブ]**&#x200B;であることを確認します。 **[!UICONTROL Installed]**&#x200B;状態のバンドルのトラブルシューティングに関する詳細は、以下を参照してください。

#### ログの確認

AEMはチャットプラットフォームで、**error.log**&#x200B;に多くの有用な情報を記録します。 **error.log**&#x200B;は、AEMがインストールされている場所にあります。&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

問題を追跡するための有効な手法は、Javaコードにログステートメントを追加することです。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

デフォルトでは、**error.log**&#x200B;は&#x200B;*[!DNL INFO]*&#x200B;文をログに記録するように設定されます。 ログレベルを変更する場合は、[!UICONTROL ログサポート]に移動して、次の手順を実行します。[http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). また、**error.log**&#x200B;がチャットしすぎる場合もあります。 [!UICONTROL ログサポート]を使用して、指定したJavaパッケージのログステートメントを設定できます。 これは、カスタムコードの問題をOOTB AEMプラットフォームの問題から簡単に分離できるように、プロジェクトのベストプラクティスです。

![AEMでのログ設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### バンドルはインストール済みの状態です{#bundle-active}

すべてのバンドル（フラグメントを除く）は、**[!UICONTROL アクティブ]**&#x200B;状態である必要があります。 コードバンドルが[!UICONTROL Installed]の状態にある場合は、解決する必要がある問題があります。 ほとんどの場合、これは依存関係の問題です。

![AEMのバンドルエラー](assets/set-up-a-local-aem-development-environment/bundle-error.png)

上のスクリーンショットでは、[!DNL WKND Core bundle]は[!UICONTROL Installed]状態です。 これは、バンドルにはAEMインスタンスで使用可能な`com.adobe.cq.wcm.core.components.models`とは異なるバージョンが必要であるためです。

使用できる便利なツールの1つは、[!UICONTROL 依存関係ファインダー]です。[http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEMインスタンスで使用可能なバージョンを調べるJavaパッケージ名を追加します。

![コアコンポーネント](assets/set-up-a-local-aem-development-environment/core-components.png)

上記の例では、AEMインスタンスにインストールされているバージョンが、バンドルが予期していた&#x200B;**12.2**&#x200B;対&#x200B;**12.6**&#x200B;であることがわかります。 そこから後方に進み、AEM上の[!DNL Maven]依存関係がAEMプロジェクト内の[!DNL Maven]依存関係と一致するかどうかを確認できます。 上記の例では、 [!DNL Core Components] **v2.2.0**&#x200B;がAEMインスタンスにインストールされていますが、コードバンドルは&#x200B;**v2.2.2**&#x200B;に依存関係を持って構築されたので、依存関係の問題が発生します。

#### Slingモデルの登録の確認{#osgi-component-sling-models}

AEMコンポーネントは、常に[!DNL Sling Model]を使用して、あらゆるビジネスロジックをカプセル化し、HTLレンダリングスクリプトをクリーンな状態に維持する必要があります。 Slingモデルが見つからない問題が発生した場合は、コンソールから[!DNL Sling Models]を確認すると役立つ場合があります。[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). これにより、Slingモデルが登録されているかどうか、およびSlingモデルが関連付けられているリソースタイプ（コンポーネントパス）が示されます。

![Slingモデルのステータス](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

[!DNL Sling Model]、`wknd/components/content/byline`のコンポーネントリソースタイプに結び付けられた`BylineImpl`の登録を表示します。

#### CSSまたはJavaScriptの問題

ほとんどのCSSおよびJavaScriptの問題に対して、ブラウザーの開発ツールを使用することが、最も効果的なトラブルシューティング方法です。 AEMオーサーインスタンスに対して開発する際の問題を絞り込むには、「公開済み」ページを表示すると便利です。

![CSSまたはJSの問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

[!UICONTROL ページのプロパティ]メニューを開き、「[!UICONTROL 公開済みとして表示]」をクリックします。 これにより、AEMエディターを使用せずにページが開き、クエリパラメーターが&#x200B;**wcmmode=disabled**&#x200B;に設定されます。 これにより、AEMオーサリングUIが効果的に無効になり、フロントエンドの問題のトラブルシューティングやデバッグがより簡単になります。

フロントエンドコードの開発時に、古い、または古いCSS/JSが読み込まれている際に、よく発生する問題がもう1つあります。 最初の手順として、ブラウザー履歴がクリアされ、必要に応じて匿名ブラウザーまたは新しいセッションを開始します。

#### クライアントライブラリのデバッグ

カテゴリや埋め込みの様々な方法で複数のクライアントライブラリを含めると、トラブルシューティングが面倒になる場合があります。 AEM はそのためにいくつかのツールを公開しています。最も重要なツールの1つは、[!UICONTROL クライアントライブラリの再構築]です。これは、AEMがLESSファイルを再コンパイルし、CSSを生成するよう強制します。

* [ライブラリのダンプ](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。&lt;host>/libs/granite/ui/content/dumplibs.html
* [出力テスト](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - カテゴリ別を含む、clientlib の予想される HTML 出力を確認できます。&lt;host>/libs/granite/ui/content/dumplibs.test.html
* [ライブラリの依存関係の検証](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 見つからない依存関係や埋め込みカテゴリをハイライトします。&lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [クライアントライブラリの再ビルド](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。&lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![clientlibのデバッグ](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>[!UICONTROL クライアントライブラリの再構築]ツールを使用してキャッシュを常に無効にする必要がある場合は、すべてのクライアントライブラリを1回再構築するだけで済みます。 この処理には約15分かかりますが、通常は将来キャッシュの問題が発生しなくなります。
