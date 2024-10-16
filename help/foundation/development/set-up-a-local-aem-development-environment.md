---
title: ローカル AEM 開発環境のセットアップ
description: Experience Manager のローカル開発環境のセットアップ方法を説明します。ローカルインストール、Apache Maven、統合開発環境およびデバッグとトラブルシューティングについて理解します。Eclipse IDE、CRXDE-Lite、Visual Studio Code および IntelliJ を使用します。
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4537
source-git-commit: d731a7131b997fa272013e8d62aa2251e25c08e4
workflow-type: ht
source-wordcount: '2423'
ht-degree: 100%

---

# ローカル AEM 開発環境のセットアップ

Adobe Experience Manager（AEM）のローカル開発のセットアップに関するガイドです。ローカルインストール、Apache Maven、統合開発環境およびデバッグ／トラブルシューティングに関する重要なトピックについて説明します。**Eclipse IDE、CRXDE Lite、Visual Studio Code および IntelliJ** を使用した開発について説明します。

## 概要

Adobe Experience Manager（AEM）向けの開発をするときの最初のステップは、ローカル開発環境のセットアップです。生産性を高め、より良いコードをより迅速に記述するために、時間をかけて質の高い開発環境をセットアップします。AEM のローカル開発環境は、次の 4 つの領域に分けることができます。

* ローカル AEM インスタンス
* [!DNL Apache Maven] プロジェクト
* 統合開発環境（IDE）
* トラブルシューティング

## ローカル AEM インスタンスのインストール

ローカル AEM インスタンスとは、開発者の個人用マシン上で動作している Adobe Experience Manager のコピーのことです。***すべて***&#x200B;の AEM 開発は、ローカル AEM インスタンスに対してコードを記述し実行するところから始まります。

AEM を初めて使用する場合は、***オーサー***&#x200B;と&#x200B;***パブリッシュ***&#x200B;という 2 つの基本的な実行モードをインストールできます。***オーサー*** [実行モード](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=ja)は、デジタルマーケターがコンテンツの作成と管理に使用する環境です。開発時には、ほとんどの場合、オーサーインスタンスにコードをデプロイします。これにより、ページを作成し、コンポーネントを追加および設定できるようになります。AEM Sites は WYSIWYG オーサリング CMS なので、ほとんどの CSS と JavaScript をオーサーインスタンスに対してテストできます。

これは、ローカルの&#x200B;***パブリッシュ***&#x200B;インスタンスに対する&#x200B;*重要な*&#x200B;テストコードでもあります。***パブリッシュ***&#x200B;インスタンスは、web サイトの訪問者がやり取りを行う AEM 環境です。***パブリッシュ***&#x200B;インスタンスは&#x200B;***オーサー***&#x200B;インスタンスと同じテクノロジースタックですが、設定と権限に関して大きな違いがあります。コードは、上位レベルの環境に昇格させる前に、ローカルの&#x200B;***パブリッシュ***&#x200B;インスタンスに対してテストする必要があります。

### 手順

1. Java™ がインストールされていることを確認します。
   * AEM 6.5 以降では [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/jp/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) を推奨します。
   * AEM 6.5 より前の AEM バージョンに対応する [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/)
1. [AEM クイックスタート Jar と [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=ja) のコピーを取得します。
1. お使いのコンピューター上に次のようなフォルダー構造を作成します。

```plain
~/aem-sdk
    /author
    /publish
```

1. [!DNL QuickStart] JAR を ***aem-author-p4502.jar*** に名称変更し、`/author` ディレクトリの下に配置します。***[!DNL license.properties]*** ファイルを `/author` ディレクトリの下に追加します。

1. [!DNL QuickStart] JAR のコピーを作成し、***aem-publish-p4503.jar*** に名称変更して、`/publish` ディレクトリの下に配置します。***[!DNL license.properties]*** ファイルのコピーを `/publish` ディレクトリの下に追加します。

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. ***aem-author-p4502.jar*** ファイルをダブルクリックして、**オーサー**&#x200B;インスタンスをインストールします。これにより、ローカルコンピューターのポート **4502** で動作するオーサーインスタンスが起動します。

***aem-publish-p4503.jar*** ファイルをダブルクリックして、**パブリッシュ**&#x200B;インスタンスをインストールします。これにより、ローカルコンピューターのポート **4503** で動作するパブリッシュインスタンスが起動します。

>[!NOTE]
>
>開発マシンのハードウェアによっては、**オーサーインスタンスとパブリッシュインスタンス**&#x200B;の両方を同時に実行することが難しい場合があります。ローカルセットアップで両方を同時に実行する必要があることは稀です。

### コマンドラインの使用

JAR ファイルをダブルクリックする代わりに、コマンドラインから AEM を起動するか、ローカルオペレーティングシステムの種類に応じてスクリプト（`.bat` または `.sh`）を作成することもできます。サンプルコマンドの例を以下に示します。

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

ここで、`-X` は JVM オプションで、`-D` は追加のフレームワークプロパティです。詳しくは、[AEM インスタンスのデプロイと保守](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=ja)および[クイックスタートファイルから使用できるその他のオプション](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html?lang=ja#further-options-available-from-the-quickstart-file)を参照してください。

## Apache Maven のインストール

***[!DNL Apache Maven]*** は、Java ベースのプロジェクトのビルドおよびデプロイ手順を管理するツールです。AEM は Java ベースのプラットフォームであり、[!DNL Maven] は AEM プロジェクトのコードを管理する標準的な方法になります。***AEM Maven プロジェクト***&#x200B;または単に ***AEM プロジェクト*** と言う場合、サイトのすべての&#x200B;*カスタム*&#x200B;コードを含む Maven プロジェクトのことを指しています。

すべての AEM プロジェクトは、**[!DNL AEM Project Archetype]** の最新バージョン（[https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype)）に基づいて構築する必要があります。[!DNL AEM Project Archetype] には、サンプルのコードとコンテンツを含んだ AEM プロジェクトのブートストラップが用意されています。[!DNL AEM Project Archetype] には、プロジェクトで使用するように設定された **[!DNL AEM WCM Core Components]** も含まれています。

>[!CAUTION]
>
>新しいプロジェクトを開始する際には、アーキタイプの最新バージョンを使用するのがベストプラクティスです。アーキタイプには複数のバージョンがあり、すべてのバージョンが以前のバージョンの AEM と互換性があるわけではないことに留意してください。

### 手順

1. [Apache Maven](https://maven.apache.org/download.cgi) をダウンロードします。
2. [Apache Maven](https://maven.apache.org/install.html) をインストールし、インストールがコマンドライン `PATH` に追加されていることを確認します。
   * [!DNL macOS] ユーザーは、[Homebrew](https://brew.sh/) を使用して Maven をインストールできます。
3. 新しいコマンドラインターミナルを開き、次のコマンドを実行することによって、**[!DNL Maven]** がインストールされていることを確認します。

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> 以前は、`nexus.adobe.com` を指定して AEM アーティファクトをダウンロードするために、`adobe-public` Maven プロファイルを追加する必要がありました。すべての AEM アーティファクトは Maven Central を通じて利用できるようになり、`adobe-public` プロファイルは必要なくなりました。

## 統合開発環境のセットアップ

統合開発環境（IDE）は、テキストエディター、構文サポートおよびビルドツールを組み合わせたアプリケーションです。実施している開発のタイプによっては、ある IDE が別の IDE よりも望ましい場合があります。IDE に関係なく、テストのためにコードをローカル AEM インスタンスに定期的に&#x200B;***プッシュ***&#x200B;できることが重要です。Git などのソース管理システムに保持するために、ローカル AEM インスタンスから AEM プロジェクトに設定を時々&#x200B;***プル***&#x200B;することが重要です。

以下は、AEM 開発で使用される一般的な IDE のいくつかと、それぞれに対応するビデオです。ビデオでは、ローカル AEM インスタンスとの統合について説明しています。

>[!NOTE]
>
> WKND プロジェクトは、デフォルトで AEM as a Cloud Service で動作するように更新されました。[6.5／6.4 との下位互換性を保つ](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)ように更新されています。AEM 6.5 または 6.4 を使用している場合は、`classic` プロファイルをすべての Maven コマンドに追加します。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDE を使用する場合は、Maven プロファイルタブで `classic` に必ずチェックを入れてください。

![Maven プロファイルタブ](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven プロファイル*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** は、Java™ 開発用の最も人気のある IDE の 1 つです。その主な理由は、オープンソースであり、***無料***&#x200B;であるからです。アドビでは、ローカル AEM インスタンスとコードを同期するための優れた GUI で開発しやすくするために、[!DNL Eclipse] 用のプラグイン **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=ja)** を提供しています。[!DNL AEM Developer Tools] が GUI をサポートいることもあり、AEM を初めて使用する開発者には [!DNL Eclipse] IDE をお勧めします。

#### インストールとセットアップ

1. [!DNL Java™ EE Developers] の [!DNL Eclipse] IDE を [https://www.eclipse.org](https://www.eclipse.org/) からダウンロードしてインストールします。
1. 指示に従って、[https:// experienceleague.adobe.com/docs/ experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=ja) から [!DNL AEM Developer Tools] プラグインをインストールします。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Maven プロジェクトの読み込み
* 01:24 - Maven を使用したソースコードのビルドとデプロイ
* 04:33 - AEM Developer Tools を使用したコード変更のプッシュ
* 10:55 - AEM Developer Tools を使用したコード変更のプル
* 13:12 - Eclipse の統合デバッグツールの使用

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)** は、プロフェッショナルな Java™ 開発のための強力な IDE です。[!DNL IntelliJ IDEA] には、***無料***&#x200B;の [!DNL Community] 版と商用（有料）の [!DNL Ultimate] 版の 2 種類があります。多くの AEM 開発には [!DNL IntellIJ IDEA] の無料の [!DNL Community] 版でも十分ですが、[!DNL Ultimate] 版では[その機能セットが拡張](https://www.jetbrains.com/idea/download)されています。

#### [!DNL Installation and Setup]

1. [!DNL IntelliJ IDEA] を [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download) からダウンロードしてインストールします。
1. [!DNL Repo]（コマンドラインツール）を [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation) からインストールします。

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Maven プロジェクトの読み込み
* 05:47 - Maven を使用したソースコードのビルドとデプロイ
* 08:17 - Repo を使用した変更のプッシュ
* 14:39 - Repo を使用した変更のプル
* 17:25 - IntelliJ IDEA の統合デバッグツールの使用

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** は、JavaScript サポートの強化版である [!DNL Intellisense] とブラウザーデバッグのサポートにより、すぐに&#x200B;***フロントエンド開発者***&#x200B;のお気に入りのツールになりました。**[!DNL Visual Studio Code]** は、多くの強力な拡張機能を備えた無料のオープンソースです。[!DNL Visual Studio Code] は、アドビのツール **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code) を使用して、AEM と連携するようにセットアップできます。** コミュニティでサポートしているいくつかの拡張機能をインストールして AEM と統合することもできます。

[!DNL Visual Studio Code] は、主に CSS／LESS や、AEM クライアントライブラリを作成するための JavaScript コードを記述するフロントエンド開発者に最適です。ただし、このツールは、ノード定義（ダイアログ、コンポーネント）を生の XML で編集する必要があるので、AEM を初めて使用する開発者には最適でない可能性があります。[!DNL Visual Studio Code] で使用できる Java™ 拡張機能はいくつかありますが、主に Java™ 開発を行う場合は、[!DNL Eclipse IDE] または [!DNL IntelliJ] が望ましいでしょう。

#### 重要なリンク

* **Visual Studio Code** の&#x200B;[**ダウンロード**](https://code.visualstudio.com/Download)
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR コンテンツ用の FTP に似たツール
* **[aemfed](https://aemfed.io/)** - AEM フロントエンドのワークフローを迅速化します
* **[AEM 同期](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - コミュニティでサポートされている&#42; Visual Studio Code 用の拡張機能
* **[WKND プロジェクト](https://github.com/adobe/aem-guides-wknd)** - このビデオで示す AEM プロジェクトの例。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Maven プロジェクトの読み込み
* 00:53 - Maven を使用したソースコードのビルドとデプロイ
* 04:03 - Repo コマンドラインツールを使用したコード変更のプッシュ
* 08:29 - Repo コマンドラインツールを使用したコード変更のプル
* 10:40 - aemfed ツールを使用したコード変更のプッシュ
* 14:24 - トラブルシューティング、クライアントライブラリの再ビルド

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html?lang=ja) は、AEM リポジトリのブラウザーベースのビューです。[!DNL CRXDE Lite] は AEM に組み込まれており、開発者はこれを使用して、ファイルの編集や、コンポーネント、ダイアログおよびテンプレートの定義など、標準的な開発タスクを実行できます。[!DNL CRXDE Lite] は完全な開発環境を意図したものでは&#x200B;***ありません***&#x200B;が、デバッグツールとして効果的です。[!DNL CRXDE Lite] は、コードベース外の製品コードを拡張したり、単に理解したりする場合に便利です。[!DNL CRXDE Lite] は、リポジトリの強力なビューを提供し、権限を効果的にテストおよび管理する手段となります。

[!DNL CRXDE Lite] は、他の IDE と一緒に使用してコードのテストとデバッグを行うべきものであり、主要な開発ツールとして使用しないでください。構文のサポートに制限があり、オートコンプリート機能はなく、ソース管理システムとの統合も制限されています。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## トラブルシューティング

***助けて。*** コードが動作しません。どのような開発でも、コードが期待どおりに動作しないことは（おそらくよく）あります。AEM は強力なプラットフォームですが、強力であればあるほど、複雑さも増します。以下は、問題のトラブルシューティングとトラッキングを行う際の概要です（ただし、問題が発生する可能性がある事項を網羅したものではありません）。

### コードのデプロイメントの検証

問題が発生した場合の最初のステップは、コードが AEM に正常にデプロイされインストールされていることを確認することです。

1. コードパッケージがアップロードされインストールされていることを、**[!UICONTROL パッケージマネージャー]**（[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)）で確認します。パッケージが最近インストールされたことを、タイムスタンプで確認します。
1. [!DNL Repo] や [!DNL AEM Developer Tools] などのツールを使用してファイルの増分更新を行う場合は、ファイルがローカル AEM インスタンスにプッシュされていることと、ファイルの内容が更新されていることを、**[!DNL CRXDE Lite]**（[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)）で確認します。
1. OSGi バンドル内の Java™ コードに関連する問題が発生した場合は、**バンドルがアップロードされていることを確認**&#x200B;します。[!UICONTROL Adobe Experience Manager web コンソール]（[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)）を開き、バンドルを検索します。バンドルのステータスが&#x200B;**[!UICONTROL アクティブ]**&#x200B;であることを確認します。**[!UICONTROL インストール済み]**&#x200B;状態にあるバンドルのトラブルシューティングについて詳しくは、以下を参照してください。

#### ログの確認

AEM は情報提供の充実したプラットフォームであり、**error.log** に有用な情報を記録しています。**error.log** は、AEM がインストールされている場所にあります（&lt;`aem-installation-folder>/crx-quickstart/logs/error.log`）。

問題を追跡するための有効な手法は、Java™ コードにログステートメントを追加することです。

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

デフォルトでは、**error.log** は *[!DNL INFO]* ステートメントをログに記録するように設定されています。ログレベルを変更したい場合は、[!UICONTROL ログサポート]（[http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)）に移動して変更できます。また、**error.log** に含まれている情報が多すぎると感じることもあるかもしれません。その場合は、[!UICONTROL ログサポート]を使用して、指定した Java™ パッケージのみのログステートメントを設定することができます。これはプロジェクトのベストプラクティスの 1 つで、カスタムコードの問題を OOTB AEM プラットフォームの問題と簡単に切り分けるためのものです。

![AEM でのログ設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### バンドルはインストール済み状態です {#bundle-active}

すべてのバンドル（フラグメントを除く）は&#x200B;**[!UICONTROL アクティブ]**&#x200B;状態になっている必要があります。コードバンドルが[!UICONTROL インストール済み]状態の場合は、解決すべき問題があります。ほとんどの場合、それは依存関係の問題です。

![AEM でのバンドルエラー](assets/set-up-a-local-aem-development-environment/bundle-error.png)

上記のスクリーンショットでは、[!DNL WKND Core bundle] は[!UICONTROL インストール済み]状態です。これは、AEM インスタンスで使用可能なバージョンとは異なるバージョンの `com.adobe.cq.wcm.core.components.models` をバンドルが想定しているからです。

使用できる便利なツールは、[!UICONTROL 依存関係ファインダー]（[http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)）です。AEM インスタンスで使用可能なバージョンを調べるには、Java™ パッケージ名を追加します。

![コアコンポーネント](assets/set-up-a-local-aem-development-environment/core-components.png)

上記の例を続けると、AEM インスタンスにインストールされているバージョンが、バンドルが想定していた **12.6** ではなく、**12.2** であることがわかります。そこからさかのぼって、AEM での [!DNL Maven] 依存関係が AEM プロジェクトの [!DNL Maven] 依存関係と一致するかどうかを確認できます。上記の例では、[!DNL Core Components] **v2.2.0** は AEM インスタンスにインストールされていますが、コードバンドルは **v2.2.2** に基づいてビルドされているので、依存関係の問題が発生します。

#### Sling モデルの登録の検証 {#osgi-component-sling-models}

AEM コンポーネントは、ビジネスロジックをカプセル化し、HTL レンダリングスクリプトがクリーンなままであることを保証するために、[!DNL Sling Model] をベースにする必要があります。Sling モデルが見つからないという問題が発生した場合は、コンソール（[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)）で [!DNL Sling Models] を確認すると役に立つ場合があります。これにより、Sling モデルが登録されているかどうかと、モデルが関連付けられているリソースタイプ（コンポーネントパス）がわかります。

![Sling モデルのステータス](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

`wknd/components/content/byline` というコンポーネントリソースタイプに関連付けられている [!DNL Sling Model] である `BylineImpl` の登録を示します。

#### CSS または JavaScript の問題

CSS や JavaScript のほとんどの問題については、ブラウザーの開発ツールを使用することが、最も効果的なトラブルシューティング方法です。AEM オーサーインスタンスに対して開発する際の問題を絞り込むには、「公開済みとして」ページを表示すると便利です。

![CSS または JS の問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

[!UICONTROL ページプロパティ]メニューを開き、「[!UICONTROL 公開済みとして表示]」をクリックします。これにより、AEM エディターを含まず、クエリパラメーターが **wcmmode=disabled** に設定されたページが開きます。これにより、AEM オーサリング UI が効果的に無効になり、フロントエンドの問題のトラブルシューティングやデバッグがはるかに容易になります。

フロントエンドコードを開発するときによく発生するもう 1 つの問題は、古い CSS や JS が読み込まれることです。最初の手順として、ブラウザー履歴がクリアされていることを確認し、必要に応じて匿名ブラウザーまたは新しいセッションを開始します。

#### クライアントライブラリのデバッグ

複数のクライアントライブラリを組み込むために、カテゴリや埋め込みの様々な方法を使用すると、トラブルシューティングが面倒になります。AEM では、そのような場合に役立つツールをいくつか公開しています。最も重要なツールの 1 つは、[!UICONTROL クライアントライブラリの再ビルド]です。これにより、AEM はすべての LESS ファイルを強制的に再コンパイルし、CSS を生成します。

* [ライブラリのダンプ](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM インスタンスに登録されているすべてのクライアントライブラリを一覧表示します。&lt;host>/libs/granite/ui/content/dumplibs.html
* [出力テスト](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - clientlib のインクルードの予想される HTML 出力をカテゴリ別に確認できます。&lt;host>/libs/granite/ui/content/dumplibs.test.html
* [ライブラリの依存関係の検証](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 見つからないすべての依存関係または埋め込みカテゴリを強調表示します。&lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [クライアントライブラリの再ビルド](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - すべてのクライアントライブラリの再ビルドまたはクライアントライブラリのキャッシュの無効化を AEM に強制的に行わせることができます。このツールは、生成された CSS を AEM に強制的に再コンパイルさせるので、LESS を使用した開発の際に効果的です。一般的に、キャッシュを無効化してからページの更新を行う方が、すべてのライブラリを再ビルドするよりも効果的です。&lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![clientlibs のデバッグ](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>[!UICONTROL クライアントライブラリの再ビルド]ツールを使用して、絶えずキャッシュを無効化する必要がある場合は、すべてのクライアントライブラリを 1 回再ビルドするだけの価値があるかもしれません。この処理には約 15 分かかりますが、実行すれば、通常は、今後発生する可能性のあるキャッシュの問題が解消されます。
