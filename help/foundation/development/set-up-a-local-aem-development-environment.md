---
title: ローカルAEM開発環境の設定
description: AEM、Adobe Experience Managerの地域開発を始めるためのガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングの重要なトピックをカバーしています。 Eclipse IDE、CRXDE-Lite、Visual Studio Code、およびIntelliJを使用した開発について説明します。
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2594'
ht-degree: 8%

---


# ローカルAEM開発環境の設定

AEM、Adobe Experience Managerの地域開発を始めるためのガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングの重要なトピックをカバーしています。 、、 **[!DNL Eclipse IDE]およびを使用した開発につい[!DNL CRXDE Lite]て説明[!DNL Visual Studio Code][!DNL IntelliJ]** します。

## 概要

Adobe Experience ManagerやAEM向けに開発する場合は、ローカル開発環境の設定が最初に行われます。 品質開発環境の設定に時間をかけて、生産性を高め、より優れたコードをより速く作成できます。 AEMのローカル開発環境を次の4つの領域に分割できます。

* ローカルAEMインスタンス
* [!DNL Apache Maven] project
* 統合開発環境(IDE)
* トラブルシューティング

## ローカルAEMインスタンスのインストール

ローカルのAEMインスタンスを指す場合、開発者のパーソナルマシン上で稼働しているAdobe Experience Managerのコピーについてお話します。 ***すべてのAEM*** 開発では、コードを書き込んでローカルのAEMインスタンスに対して実行することで開始を行う必要があります。

AEMを初めて使用する場合は、次の2つの基本的な実行モードをインストールできます。 ***作成者*** / ***発行***。 「 ***作成者*** 」 [](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) 実行モードは、デジタルマーケターがコンテンツの作成と管理に使用する環境です。 開発を進める **ときは** 、ほとんどの場合、作成者インスタンスにコードをデプロイします。 これにより、新しいページを作成したり、コンポーネントを追加および設定したりできます。 AEM SitesはWYSIWYGオーサリングCMSなので、CSSとJavaScriptのほとんどはオーサリングインスタンスに対してテストできます。

また、ローカルの *発行インスタンスに対する* 重要な ****** テストコードです。 発行 ***インスタンスは*** 、Webサイトの訪問者がやり取りするAEM環境です。 発行 ***インスタンスは*** 、 ****** 作成者インスタンスと同じテクノロジースタックですが、設定と権限に関しては主な違いがあります。 高いレベルの環境に昇格する前に *、コードを* 常に ***、ローカルの*** 発行インスタンスに対してテストする必要があります。

### 手順

1. [Javaがインストールされていることを確認します](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) 。
   * AEM [6.5以上にはJava JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastModified&amp;by.sort=&amp;layout=リスト&amp;p.offset=0&amp;p.limit=14) を優先
   * [AEM 6.5より前のAEMバージョン用Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 8
2. AEM QuickStart Jarのコピーと [を取得します [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)。
3. 次のようなフォルダー構造をコンピューター上に作成します。

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. この [!DNL QuickStart] JARの名前を ***aem-author-p4502.jarに変更し、***`/author` ディレクトリの下に配置します。 デ追加ィレクトリの下のフ ***[!DNL license.properties]***`/author` ァイル。
5. JARのコピーを作成し、 [!DNL QuickStart] JARの名前を ***aem-publish-p4503.jarに変更して、***`/publish` ディレクトリの下に配置します。 ディレ追加クトリの下にある ***[!DNL license.properties]*** ファイルのコピー `/publish` です。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 重複が ***aem-author-p4502.jarファイルをクリックして、*** 作成者 **** インスタンスをインストールします。 これにより、ローカルコンピューターのポート4502 **で実行されている作成者インスタンスが開始されます** 。

   重複が ***aem-publish-p4503.jarファイルをクリックして、*** 発行 **** インスタンスをインストールします。 これにより、ローカルコンピューターのポート4503 **で実行されている発行インスタンスが開始され** ます。

   >[!NOTE]
   >
   >開発マシンのハードウェアによっては、 **作成者インスタンスと発行** インスタンスの両方を同時に実行するのが難しい場合があります。 ローカルセットアップで同時に両方を実行する必要がある場合はほとんどありません。

   詳しくは、AEMインスタンスの [デプロイと保守を参照してください](https://helpx.adobe.com/ja/experience-manager/6-5/sites/deploying/using/deploy.html)。

## Apache Mavenのインストール

***[!DNL Apache Maven]*** は、Javaベースのプロジェクトの構築と展開の手順を管理するツールです。 AEMは、Javaベースのプラットフォームで、AEMプロジェクトのコード [!DNL Maven] を管理する標準的な方法です。 ***AEM Maven Project*** 、または ***AEM Project***&#x200B;のみと言うと、サイトのすべての ** カスタムコードを含むMavenプロジェクトを指します。

すべてのAEMプロジェクトは、次の最新バージョンで構築する必要があり **[!DNL AEM Project Archetype]**&#x200B;ます。 [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). では、サンプルコード [!DNL AEM Project Archetype] とコンテンツを含むAEMプロジェクトのブートストラップを作成します。 には、プロジェクトで使用す [!DNL AEM Project Archetype]**[!DNL AEM WCM Core Components]** るように設定されたものも含まれます。

>[!CAUTION]
>
>新しいプロジェクトを開始する場合は、最新バージョンのアーキタイプを使用することをお勧めします。 アーキタイプには複数のバージョンがあり、すべてのバージョンが旧バージョンのAEMと互換性があるわけではありません。

### 手順

1. Apache [Mavenのダウンロード](https://maven.apache.org/download.cgi)
2. [Apache Mavenをインストールし](https://maven.apache.org/install.html) 、インストールがコマンドラインに追加されていることを確認し `PATH`ます。
   * [!DNL macOS] ユーザーは、 [Homebrewを使用してMavenをインストールできます。](https://brew.sh/)
3. 新しいコマンドラインターミナル **[!DNL Maven]** を開き、次のコマンドを実行して、がインストールされていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. maven構築プロセス追加に自動的に追加するための **[!DNL adobe-public]** プロファイル [!DNL Maven] ( [settings.xml](https://maven.apache.org/settings.html)**[!DNL repo.adobe.com]** ファイルへの)。

5. Create a file named `settings.xml` at `~/.m2/settings.xml` if it doesn&#39;t exist already.

6. Add the **[!DNL adobe-public]** profile to the `settings.xml` file based on [the instructions here](https://repo.adobe.com/).

   A sample `settings.xml` is listed below. *の命名規則`settings.xml`と、ユーザーのディレクトリの下の配置が重要`.m2`です。*

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

7. 次のコマンドを実行して、 **adobe-public** プロファイルがアクティブであることを確認します。

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

   表示されない場合は、 **[!DNL adobe-public]** Adobeレポートが `~/.m2/settings.xml` ファイル内で正しく参照されていないことを示しています。 前の手順を再度実行し、settings.xmlファイルがAdobeレポートを参照していることを確認してください。

## 統合開発環境の設定

統合開発環境(IDE)は、テキストエディタ、構文サポート、および構築ツールを組み合わせたアプリケーションです。 開発の種類に応じて、IDEの方が他のIDEよりも優れている場合があります。 IDEに関係なく、ローカルAEMインスタンスに定期的に ***コードを*** プッシュして、テストすることが重要です。 また、Gitなどのソース管理システムに持続させるために、ローカルのAEMインスタンスからAEMプロジェクトに ***設定を*** 引き出すことも重要です。

次に、ローカルAEMインスタンスとの統合を示す対応ビデオを含むAEM開発で使用される、より一般的なIDEのいくつかを示します。

### [!DNL Eclipse] IDE

IDE **[[!DNL Eclipse] はJava開発に最も人気の高いIDEの一つです。IDEはオープンソースで](https://www.eclipse.org/ide/)** フリーなので、大部分は ******! Adobeは、優れたGUIを使用した開発を容易にして、コードをローカルのAEMインスタンスと同期でき **[るように、プラグイン[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**[!DNL Eclipse] を提供しています。 IDEは、によるGUIサポートのため、AEMを初めて使用する開発者の大部分に推奨され [!DNL Eclipse][!DNL AEM Developer Tools]ます。

#### インストールとセットアップ

1. IDEをダウンロードしてインストールし [!DNL Eclipse] ま [!DNL Java EE Developers]す。 [https://www.eclipse.org](https://www.eclipse.org/)
1. 指示に従ってプラグインをインストールし [!DNL AEM Developer Tools] ます。 [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Mavenプロジェクトのインポート
* 01:24 - Mavenを使用したソースコードの構築とデプロイ
* 04:33 - AEM Developer Toolでのプッシュコードの変更
* 10:55 - AEM Developer Toolでのプルコード変更
* 13:12 - Eclipseの統合デバッグツールの使用

### IntelliJ IDEA

IntelliJ IDEA **[は、Javaの専門的な開発に役立つ強力なIDEです](https://www.jetbrains.com/idea/)** 。 [!DNL IntelliJ IDEA] フリー ***版とコマーシャル版の2種類***[!DNL Community][!DNL Ultimate] があります。 の無料 [!DNL Community] バージョンは、より多くのAEM開発を行うには十分です [!DNL IntellIJ IDEA] が、機能セット [!DNL Ultimate] は拡張されています [](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 次のファイルをダウンロードしてインストールし [!DNL IntelliJ IDEA]ます。 [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. インストール [!DNL Repo] （コマンドラインツール）: [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Mavenプロジェクトのインポート
* 05:47 - Mavenを使用したソースコードの構築とデプロイ
* 08:17 — リポを使用したプッシュの変更
* 14:39 — リポで変更を取り込む
* 17:25 - IntelliJ IDEAの統合デバッグツールの使用

### [!DNL Visual Studio Code]

**[強化されたJavaScriptのサポート、ブラウザのデバッグサポートを含む、フロン](https://code.visualstudio.com/)** トエンド開発者のお気に入りのツール ***、Visual Studio Code***[!DNL Intellisense]がすぐに使えるようになりました。 **[!DNL Visual Studio Code]** オープンソースで、無料で、多くの強力な拡張機能を持つ。 [!DNL Visual Studio Code] は、Adobeツール **[repoを使用してAEMとの統合を設定でき](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)ます。** AEMと統合するためにインストールできる、コミュニティでサポートされる拡張機能もいくつかあります。

[!DNL Visual Studio Code] は、主にAEMクライアントライブラリを作成するためにCSS/LESSとJavaScriptコードを記述するフロントエンド開発者に最適です。 ノード定義（ダイアログやコンポーネント）はすべて生のXMLで編集する必要があるので、新しいAEM開発者にとってこのツールは最適な選択ではないかもしれません。 で使用できるJava拡張機能はいくつかあります [!DNL Visual Studio Code]が、主にJava開発を行う場合や、推奨される場合 [!DNL Eclipse IDE] があ [!DNL IntelliJ] ります。

#### 重要なリンク

* [**Visual**](https://code.visualstudio.com/Download) **Studioコードのダウンロード**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCRコンテンツ用のFTPに似たツール
* **[aemfed](https://aemfed.io/)** - AEMフロントエンドワークフローの高速化
* **[AEM同期](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** — コミュニティでサポートされる* Visual Studioコードの拡張機能

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Mavenプロジェクトのインポート
* 00:53 - Mavenを使用したソースコードの構築とデプロイ
* 04:03 — リポコマンドラインツールを使用したプッシュコードの変更
* 08:29 — リポコマンドラインツールを使用したプルコードの変更
* 10:40 — 埋め込みツールを使用したプッシュコードの変更
* 14:24 — トラブルシューティング、クライアントライブラリの再構築

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) は、AEMリポジトリのブラウザーベースの表示です。 [!DNL CRXDE Lite] はAEMに組み込まれており、開発者はファイルの編集、コンポーネント、ダイアログ、テンプレートの定義など、標準的な開発タスクを実行できます。 [!DNL CRXDE Lite] は完全な開発環境 ***を目的としたものではありませんが*** 、デバッグツールとして非常に効果的です。 [!DNL CRXDE Lite] は、コードベース外の製品コードを拡張する場合や、単に理解する場合に役立ちます。 [!DNL CRXDE Lite] は、リポジトリの強力な表示と、権限を効果的にテストおよび管理する方法を提供します。

[!DNL CRXDE Lite] は、コードのテストとデバッグを行うために、常に他のIDEと組み合わせて使用する必要がありますが、主な開発ツールとしては使用できません。 構文のサポートが制限され、オートコンプリート機能がなく、ソース管理システムとの統合も限られています。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## トラブルシューティング

***ヘルプ!*** コードが機能していません！ すべての開発環境と同様に、コードが期待どおりに機能しない場合も（おそらく多く）あります。 AEMは強力なプラットフォームですが、強力なパワーを持つことで、非常に複雑になります。 以下に、問題のトラブルシューティングとトラッキングの要点をいくつか示します(ただし、問題が発生する可能性がある問題の完全なリストとは程遠い)。

### コードの導入の検証

問題が発生した場合は、まずコードがAEMに正しくデプロイおよびインストールされていることを確認します。

1. **「[!UICONTROL Package Manager]** 」をチェックし、コードパッケージがアップロードおよびインストールされていることを確認します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). タイムスタンプを確認して、パッケージが最近インストールされたことを確認します。
1. またはなどのツールを使用してファイルを増分更新する場合 [!DNL Repo] は、ファイルがローカルのAEMインスタンスにプッシュされているこ [!DNL AEM Developer Tools]とと、ファイルの内容が更新されていることを **[!DNL CRXDE Lite]** 確認します。 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **OSGiバンドル内のJavaコードに関連する問題が見つかる場合は** 、バンドルがアップロードされていることを確認します。 [ [!UICONTROL Adobe Experience ManagerWebコンソール]]を開きます。 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) and search for your bundle. バンドルのステータスが **[!UICONTROL Active]** （アクティブ）であることを確認します。 インストール済み状態のバンドルのトラブルシューティングに関する詳細は、以下を参照して **[!UICONTROL ください]** 。

#### ログの確認

AEMはチャットプラットフォームで、多くの役に立つ情報を **error.logに記録します**。 AEMがインストールされている **場所は、次のerror.logを参照してください** 。&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

問題を追跡するのに便利な方法は、Javaコードにログ文を追加することです。

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

デフォルトでは、 **error.log** はログ *[!DNL INFO]* 文に設定されます。 ログレベルを変更する場合は、 [!UICONTROL ログサポートに移動して変更できます]。 [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). また、 **error.logが機能しすぎる場合もあります** 。 ログサポートを使用すると、指定したJavaパッケージの [!UICONTROL ログ文を設定できます] 。 これは、カスタムコードの問題をOOTB AEMプラットフォームの問題から簡単に分離するための、プロジェクトのベストプラクティスです。

![AEMでのログの設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### バンドルはインストール済み状態です {#bundle-active}

すべてのバンドル（フラグメントを除く）は、 **[!UICONTROL アクティブ]** 状態にする必要があります。 コードバンドルが「 [!UICONTROL インストール済み] 」の状態になっている場合は、解決する必要がある問題があります。 ほとんどの場合、依存関係の問題です。

![AEMでのバンドルエラー](assets/set-up-a-local-aem-development-environment/bundle-error.png)

上のスクリーンショットでは、 [!DNL WKND Core bundle] は [!UICONTROL インストール済み] 状態です。 これは、バンドルが予期しているのはAEMインスタンスで使用でき `com.adobe.cq.wcm.core.components.models` るバージョンとは異なるバージョンであるためです。

使用できる便利なツールは、 [!UICONTROL 依存関係ファインダ]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEM追加インスタンスで使用可能なバージョンを調べるJavaパッケージ名。

![コアコンポーネント](assets/set-up-a-local-aem-development-environment/core-components.png)

上記の例を続けて、AEMインスタンスにインストールされているバージョンが、バンドルが予期していた12.2 **と12.6** の **** 違いであることがわかります。 ここからは後方に作業し、AEMの依存関係がAEMプロジェクトの依存関係と一致しているかどうかを確認することがで [!DNL Maven][!DNL Maven] きます。 上記の例では、 [!DNL Core Components] v2.2.0 **がAEMインスタンスにインストールされていますが** 、コードバンドルは **** v2.2.2への依存関係を持って構築されているので、依存関係の問題が発生した理由です。

#### Slingモデルの登録の確認 {#osgi-component-sling-models}

AEMコンポーネントは、ビジネスロジックをカプセル化 [!DNL Sling Model] し、HTLレンダリングスクリプトをクリーンな状態に保つために、常にによってバックアップする必要があります。 Slingモデルが見つからない問題が発生した場合は、コンソール [!DNL Sling Models] からをチェックすると便利です。 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Slingモデルが登録済みかどうか、およびSlingモデルが関連付けられているリソースタイプ（コンポーネントパス）が表示されます。

![Slingモデルの状態](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

コンポーネントリソースタイプの [!DNL Sling Model]に関連付け `BylineImpl` られている、の登録を表示 `wknd/components/content/byline`します。

#### CSSまたはJavaScriptの問題

CSSとJavaScriptのほとんどの問題のトラブルシューティングを行うには、ブラウザーの開発ツールを使用するのが最も効果的です。 AEM作成者インスタンスに対して開発する際に問題を絞り込むには、「発行済みとして」ページを表示すると便利です。

![CSSまたはJSの問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

[!UICONTROL ページプロパティ] メニューを開き、「 [!UICONTROL 表示は発行済み]」をクリックします。 これにより、AEMエディタを使用せずにページが開き、クエリパラメータが **wcmmode=disabledに設定され**&#x200B;ます。 これにより、AEMオーサリングUIが効果的に無効になり、フロントエンドの問題のトラブルシューティング/デバッグがより簡単になります。

フロントエンドコードの開発時に、古い、または古いCSS/JSが読み込まれている場合に、よく発生する問題がもう1つあります。 最初の手順として、ブラウザー履歴がクリアされ、必要に応じて匿名ブラウザーまたは新規開始がクリアされていることを確認します。

#### クライアントライブラリのデバッグ

カテゴリや埋め込みの方法が異なれば、複数のクライアントライブラリを含めることができます。これはトラブルシューティングが困難になる場合があります。 AEM はそのためにいくつかのツールを公開しています。One of the most important tools is [!UICONTROL Rebuild Client Libraries] which will force AEM to re-compile any LESS files and generate the CSS.

* [Libsのダンプ](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [出力テスト](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - カテゴリ別を含む、clientlib の予想される HTML 出力を確認できます。&lt;host>/libs/granite/ui/content/dumplibs.test.html
* [ライブラリ依存関係の検証](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) — 見つからない依存関係や埋め込みカテゴリを強調表示します。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [クライアントライブラリの再ビルド](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。&lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Clientlibのデバッグ](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Rebuild Client Libraries [!UICONTROL (クライアントライブラリの] 再構築)ツールを使用してキャッシュを常に無効にする必要がある場合は、1回だけすべてのクライアントライブラリを再構築する必要があります。 これには約15分かかりますが、通常は将来キャッシュの問題が発生しなくなります。
