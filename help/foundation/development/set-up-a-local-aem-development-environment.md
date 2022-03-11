---
title: ローカル AEM 開発環境の設定
description: Adobe Experience Manager、AEM用のローカル開発の設定に関するガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングに関する重要なトピックについて説明します。 Eclipse IDE、CRXDE-Lite、Visual Studio Code、IntelliJ を使用した開発について説明します。
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 8%

---

# ローカル AEM 開発環境の設定

Adobe Experience Manager、AEM用のローカル開発の設定に関するガイド。 ローカルインストール、Apache Maven、統合開発環境、デバッグ/トラブルシューティングに関する重要なトピックについて説明します。 を使用した開発 **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] および[!DNL IntelliJ]** を参照してください。

## 概要

Adobe Experience ManagerまたはAEM向けにローカル開発を開発する際は、開発環境の設定が最初のステップです。 生産性を高め、より高速なコードを記述するには、品質の高い開発環境の設定に時間をかけます。 AEMローカル開発環境は、次の 4 つの領域に分割できます。

* ローカルAEMインスタンス
* [!DNL Apache Maven] project
* 統合開発環境 (IDE)
* トラブルシューティング

## ローカルのAEM Instances のインストール

ローカルのAEMインスタンスを指す場合、開発者のパーソナルマシン上で実行されているAdobe Experience Managerのコピーについて説明します。 ***すべて*** AEMの開発は、まず、ローカルAEMインスタンスに対してコードを記述し、実行することから始める必要があります。

AEMを初めて使用する場合は、次の 2 つの基本的な実行モードをインストールできます。 ***作成者*** および ***公開***. この ***作成者*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  は、デジタルマーケターがコンテンツの作成と管理に使用する環境です。 開発時 **最も多い** 作成者インスタンスにコードをデプロイする時間。 これにより、新しいページを作成したり、コンポーネントを追加および設定したりできます。 AEM Sitesは WYSIWYG オーサリング CMS なので、ほとんどの CSS と JavaScript をオーサリングインスタンスに対してテストできます。

また、 *重要な* ローカルに対するテストコード ***公開*** インスタンス。 この ***公開*** インスタンスは、web サイトの訪問者がやり取りするAEM環境です。 また、 ***公開*** インスタンスは、 ***作成者*** 例えば、設定と権限には大きな違いがあります。 コードは *常に* 地元の人々に対して試験を受ける ***公開*** インスタンスを上位の環境に昇格する前に使用します。

### 手順

1. 確認 [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) がインストールされている。
   * 優先 [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (AEM 6.5 以降 )
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) AEM 6.5 より前のAEMバージョンの場合
2. のコピーを取得 [AEM QuickStart Jar と [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. 次のようなフォルダー構造をコンピューター上に作成します。

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 名前を変更 [!DNL QuickStart] JAR の宛先 ***aem-author-p4502.jar*** そしてそれをの下に置く `/author` ディレクトリ。 を ***[!DNL license.properties]*** の下のファイル `/author` ディレクトリ。
5. コピーを作成 [!DNL QuickStart] JAR、名前をに変更します。 ***aem-publish-p4503.jar*** そしてそれをの下に置く `/publish` ディレクトリ。 のコピーを追加 ***[!DNL license.properties]*** の下のファイル `/publish` ディレクトリ。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 次をダブルクリックします。 ***aem-author-p4502.jar*** インストールするファイル **作成者** インスタンス。 これにより、ポートで実行されたオーサーインスタンスが起動します **4502** ローカルコンピューター上。

   次をダブルクリックします。 ***aem-publish-p4503.jar*** インストールするファイル **公開** インスタンス。 これにより、パブリッシュインスタンスが起動し、ポートで実行されます **4503** ローカルコンピューター上。

   >[!NOTE]
   >
   >開発マシンのハードウェアによっては、 **オーサーとパブリッシュ** インスタンスが同時に実行されている。 ローカルセットアップで同時に両方を実行する必要が生じることはほとんどありません。

   詳しくは、 [AEMインスタンスのデプロイと保守](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Apache Maven のインストール

***[!DNL Apache Maven]*** は、Java ベースのプロジェクトのビルドおよびデプロイ手順を管理するツールです。 AEMは、Java ベースのプラットフォームであり、 [!DNL Maven] は、AEMプロジェクトのコードを管理する標準的な方法です。 我々が ***AEM Maven Project*** または ***AEM Project***&#x200B;を使用する場合、 *カスタム* サイトのコード。

すべてのAEMプロジェクトは、最新バージョンの **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). この [!DNL AEM Project Archetype] では、サンプルコードとコンテンツを含んだAEMプロジェクトのブートストラップを作成します。 この [!DNL AEM Project Archetype] 次を含む **[!DNL AEM WCM Core Components]** プロジェクトで使用するように設定されています。

>[!CAUTION]
>
>新しいプロジェクトを開始する際には、最新バージョンのアーキタイプを使用することをお勧めします。 アーキタイプには複数のバージョンがあり、すべてのバージョンが以前のバージョンのAEMと互換性があるわけではないことに注意してください。

### 手順

1. ダウンロード [Apache Maven](https://maven.apache.org/download.cgi)
2. インストール [Apache Maven](https://maven.apache.org/install.html) をクリックし、インストールがコマンドラインに追加されていることを確認します。 `PATH`.
   * [!DNL macOS] ユーザーは、 [Homebrew](https://brew.sh/)
3. 確認する **[!DNL Maven]** は、新しいコマンドラインターミナルを開き、次のコマンドを実行することによってインストールされます。

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
   > 以前は、 `adobe-public` Maven プロファイルは、 `nexus.adobe.com` をクリックしてAEMアーティファクトをダウンロードします。 すべてのAEMアーティファクトは、Maven Central および `adobe-public` プロファイルは不要です。

## 統合開発環境の設定

統合開発環境または IDE は、テキストエディタ、構文サポート、および build-tools を組み合わせたアプリケーションです。 実行している開発の種類によっては、ある IDE が別の IDE よりも優れている場合があります。 IDE に関係なく、定期的に実行できることが重要です ***プッシュ*** ローカルのAEMインスタンスにコードを追加してテストします。 また、 ***取り込む*** Git などのソース管理システムに保持するために、ローカルAEMインスタンスからAEMプロジェクトに設定します。

ローカルAEMインスタンスとの統合を示す、対応するビデオを含むAEM開発で使用される、より一般的な IDE の例を以下に示します。

>[!NOTE]
>
> WKND プロジェクトが、AEM as a Cloud Serviceで動作するようにデフォルトに更新されました。 更新されました [6.5/6.4 との下位互換性](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). AEM 6.5 または 6.4 を使用している場合、 `classic` 任意の Maven コマンドに対するプロファイル。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

IDE を使用する場合は、必ず `classic` を設定します。

![「Maven プロファイル」タブ](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven プロファイル*

### [!DNL Eclipse] IDE

この **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** は Java 開発用のより一般的な IDE の 1 つで、大部分はオープンソースで、 ***無料***! Adobeは、 **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**&#x200B;の場合は [!DNL Eclipse] 優れた GUI での開発を容易にし、コードをローカルのAEMインスタンスと同期させる。 この [!DNL Eclipse] IDE は、が GUI をサポートしているので、AEMを初めて使用する開発者には大きく推奨されます。 [!DNL AEM Developer Tools].

#### インストールとセットアップ

1. をダウンロードしてインストールする [!DNL Eclipse] の IDE [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. 指示に従って、 [!DNL AEM Developer Tools] プラグイン： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Maven プロジェクトの読み込み
* 01:24 - Maven を使用してソースコードをビルドしデプロイする
* 04:33 - AEM Developer Tool を使用したプッシュコードの変更
* 10:55 - AEM Developer Tool を使用してコード変更をプルします。
* 13:12 - Eclipse の統合デバッグツールの使用

### IntelliJ IDEA

この **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** は、Java のプロフェッショナル開発用の強力な IDE です。 [!DNL IntelliJ IDEA] 2 種類の味が付く。 ***無料*** [!DNL Community] 版と商業版（有料） [!DNL Ultimate] バージョン。 無料 [!DNL Community] のバージョン [!DNL IntellIJ IDEA] は、より多くのAEM開発に十分ですが、 [!DNL Ultimate] [機能セットを展開します。](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. をダウンロードしてインストールする [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. インストール [!DNL Repo] （コマンドラインツール）: [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Maven プロジェクトの読み込み
* 05:47 - Maven を使用してソースコードをビルドしデプロイする
* 08:17 — リポジトリで変更をプッシュ
* 14:39 - Repo を使用して変更をプルする
* 17:25 - IntelliJ IDEA の統合デバッグツールの使用

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** はすぐに～の好きな道具になっている ***フロントエンド開発者*** 強化された JavaScript サポートを使用する [!DNL Intellisense]、およびブラウザーのデバッグサポート。 **[!DNL Visual Studio Code]** は、多くの強力な拡張機能を備えた、無料のオープンソースです。 [!DNL Visual Studio Code] は、Adobeツールを使用してAEMと統合するように設定できます。 **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** また、AEMと統合するためにインストールできる、コミュニティでサポートされる拡張機能もいくつかあります。

[!DNL Visual Studio Code] は、主に CSS/LESS と JavaScript コードを記述してAEMクライアントライブラリを作成するフロントエンド開発者に最適です。 ノード定義（ダイアログ、コンポーネント）はすべて raw XML で編集する必要があるので、このツールは新しいAEM開発者にとって最適な選択肢ではない可能性があります。 では、複数の Java 拡張機能を使用できます。 [!DNL Visual Studio Code]ただし、主に Java 開発を行う場合は [!DNL Eclipse IDE] または [!DNL IntelliJ] の方が望ましい場合もあります。

#### 重要なリンク

* [**ダウンロード**](https://code.visualstudio.com/Download) **Visual Studio Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR コンテンツ用の FTP に似たツール
* **[aemfed](https://aemfed.io/)** - AEMフロントエンドワークフローのスピードアップ
* **[AEM同期](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Visual Studio Code 用のコミュニティサポート*拡張機能

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Maven プロジェクトの読み込み
* 00:53 - Maven でソースコードをビルドしてデプロイする
* 04:03 - Repo コマンドラインツールを使用したプッシュコードの変更
* 08:29 - Repo コマンドラインツールを使用したコードの変更のプル
* 10:40 - aemfed ツールを使用したプッシュコードの変更
* 14:24 — トラブルシューティング、クライアントライブラリの再構築

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) は、AEMリポジトリのブラウザーベースのビューです。 [!DNL CRXDE Lite] はAEMに埋め込まれており、開発者は、ファイルの編集、コンポーネント、ダイアログ、テンプレートの定義など、標準的な開発タスクを実行できます。 [!DNL CRXDE Lite] が ***not*** は完全な開発環境であることを目的としていますが、デバッグツールとして非常に効果的です。 [!DNL CRXDE Lite] は、を拡張する場合や、コードベース外の製品コードを単に理解する場合に役立ちます。 [!DNL CRXDE Lite] は、リポジトリを強力に表示し、権限を効果的にテストおよび管理する方法を提供します。

[!DNL CRXDE Lite] は、常に他の IDE と組み合わせてコードのテストとデバッグを行う必要がありますが、主な開発ツールとしては使用しないでください。 構文のサポートは制限され、オートコンプリート機能はなく、ソース管理システムとの統合も制限されます。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## トラブルシューティング

***ヘルプ!*** コードが機能しません。 すべての開発と同様に、コードが期待どおりに動作しない（おそらく多くの）場合があります。 AEMは強力なプラットフォームですが、大きな力を持ち、非常に複雑になります。 問題のトラブルシューティングとトラッキングに関する概要を以下に示します（ただし、問題が発生する可能性のあるすべての情報のリストからは程遠いものです）。

### コードのデプロイメントを検証

問題が発生した場合は、まず、コードがAEMに正常にデプロイおよびインストールされていることを確認します。

1. **チェック [!UICONTROL パッケージマネージャー]** コードパッケージがアップロードされ、インストールされていることを確認するには、次の手順を実行します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). タイムスタンプを調べて、パッケージが最近インストールされたことを確認します。
1. 次のようなツールを使用して増分ファイル更新を行う場合 [!DNL Repo] または [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** ファイルがローカルのAEMインスタンスにプッシュされ、ファイルの内容が更新されたことを示します。 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **バンドルがアップロードされたことを確認します。** OSGi バンドル内の Java コードに関する問題が発生する場合。 を開きます。 [!UICONTROL Adobe Experience Manager Web コンソール]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) お使いのバンドルを検索します。 バンドルに **[!UICONTROL アクティブ]** ステータス。 内のバンドルのトラブルシューティングに関する詳細については、以下を参照してください。 **[!UICONTROL インストール済み]** 状態。

#### ログを確認する

AEMはチャットプラットフォームで、多くの有用な情報を **error.log**. この **error.log** は、AEMのインストール先にあります。&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

問題を追跡するための有効な手法は、Java コードにログステートメントを追加することです。

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

デフォルトでは、 **error.log** をログに記録するように設定 *[!DNL INFO]* ステートメント。 ログレベルを変更する場合は、次の手順で行います： [!UICONTROL ログのサポート]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). また、 **error.log** おしゃべりすぎる 以下を使用して、 [!UICONTROL ログのサポート] 指定した Java パッケージのログステートメントを設定する場合。 これは、カスタムコードの問題を OOTB AEMプラットフォームの問題と簡単に分けるための、プロジェクトのベストプラクティスです。

![AEMでの設定のログ](./assets/set-up-a-local-aem-development-environment/logging.png)

#### バンドルはインストール済み状態です {#bundle-active}

すべてのバンドル（フラグメントを除く）は、 **[!UICONTROL アクティブ]** 状態。 コードバンドルが [!UICONTROL インストール済み] 状態の場合は、解決する必要がある問題があります。 ほとんどの場合、依存関係の問題です。

![AEMでのバンドルエラー](assets/set-up-a-local-aem-development-environment/bundle-error.png)

上のスクリーンショットでは、 [!DNL WKND Core bundle] は [!UICONTROL インストール済み] 状態。 これは、バンドルがの別のバージョンを期待しているためです `com.adobe.cq.wcm.core.components.models` がAEMインスタンスで使用可能な場合。

使用できる便利なツールは、 [!UICONTROL 依存関係ファインダー]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEMインスタンスで使用可能なバージョンを調べる Java パッケージ名を追加します。

![コアコンポーネント](assets/set-up-a-local-aem-development-environment/core-components.png)

上記の例を続けて、AEMインスタンスにインストールされているバージョンが次のようになっていることがわかります。 **12.2** vs **12.6** この包みが予期していた ここから、後方に作業して、 [!DNL Maven] AEMの依存関係が一致する [!DNL Maven] AEMプロジェクト内の依存関係。 上記の例では、 [!DNL Core Components] **v2.2.0** はAEMインスタンスにインストールされていますが、コードバンドルはに依存して構築されています。 **v2.2.2**（依存関係の問題の理由）

#### Sling モデルの登録の検証 {#osgi-component-sling-models}

AEMコンポーネントは、常に [!DNL Sling Model] ：あらゆるビジネスロジックをカプセル化し、HTL レンダリングスクリプトをクリーンなままにします。 Sling モデルが見つからない問題が発生した場合は、 [!DNL Sling Models] コンソールから： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). これにより、Sling モデルが登録されているかどうか、および関連付けられているリソースタイプ（コンポーネントパス）がわかります。

![Sling モデルのステータス](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

登録を表示します [!DNL Sling Model], `BylineImpl` コンポーネントのリソースタイプ ( `wknd/components/content/byline`.

#### CSS または JavaScript の問題

CSS や JavaScript のほとんどの問題に対して、ブラウザーの開発ツールを使用することが、最も効果的なトラブルシューティング方法です。 AEMオーサーインスタンスに対して開発する際に問題を絞り込むには、「公開済みとして」ページを表示すると便利です。

![CSS または JS の問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

を開きます。 [!UICONTROL ページプロパティ] メニューとクリック [!UICONTROL 公開済みとして表示]. これにより、AEMエディターが表示されず、クエリパラメーターがに設定された状態でページが開きます。 **wcmmode=disabled**. これにより、AEMオーサリング UI が効果的に無効になり、フロントエンドの問題のトラブルシューティングやデバッグがより簡単になります。

フロントエンドコードの開発時に発生する別の一般的な問題として、古い、または古い CSS/JS が読み込まれている場合があります。 最初の手順として、ブラウザー履歴がクリアされていることを確認し、必要に応じて匿名ブラウザーまたは新しいセッションを開始します。

#### クライアントライブラリのデバッグ

カテゴリや埋め込みの様々な方法で複数のクライアントライブラリを含めると、トラブルシューティングが面倒になる場合があります。 AEM はそのためにいくつかのツールを公開しています。最も重要なツールの 1 つは、 [!UICONTROL クライアントライブラリの再構築] これにより、AEMはすべての LESS ファイルを再コンパイルし、CSS を生成します。

* [ライブラリのダンプ](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [出力テスト](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - カテゴリ別を含む、clientlib の予想される HTML 出力を確認できます。&lt;host>/libs/granite/ui/content/dumplibs.test.html
* [ライブラリ依存関係の検証](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 見つからない依存関係または埋め込みカテゴリを強調表示します。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [クライアントライブラリの再ビルド](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。&lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![clientlib のデバッグ](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>常に [!UICONTROL クライアントライブラリの再構築] ツールすべてのクライアントライブラリを 1 回再構築する価値がある場合があります。 この処理には約 15 分かかりますが、通常は将来のキャッシュの問題が解消されます。
