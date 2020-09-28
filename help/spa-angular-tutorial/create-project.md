---
title: SPAエディタプロジェクト | AEM SPAエディターとAngularの使用の手引き
description: AEM SPA Editorと統合されたAngularアプリケーションの起点として、Adobe Experience Manager(AEM) Mavenプロジェクトを使用する方法を学びます。
sub-product: サイト
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 4%

---


# SPAエディタプロジェクト {#create-project}

AEM SPA Editorと統合されたAngularアプリケーションの起点として、Adobe Experience Manager(AEM) Mavenプロジェクトを使用する方法を学びます。

## 目的

1. Mavenアーキタイプから構築された新しいAEM SPA Editorプロジェクトの構造を理解します。
2. スタータープロジェクトをAEMのローカルインスタンスにデプロイします。

## 作成する内容

この章では、 [AEMプロジェクトのアーキタイプに基づいて新しいAEMプロジェクトを導入します](https://github.com/adobe/aem-project-archetype)。 AEMプロジェクトは、Angular SPAの非常に単純な開始点でブートストラップされます。 本章で使用するプロジェクトは、WKND SPAの導入の基盤となり、今後の章で構築される予定です。

![WKND SPA角度スタータープロジェクト](./assets/create-project/what-you-will-build.png)

*古典的な「ハローワールド」メッセージです。*

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment). オーサー **モードで開始したAdobe Experience Managerの新しいインスタンスが、ローカルで実行されていることを確認し** ます。

## プロジェクトの取得

AEM用のMavenマルチモジュールプロジェクトを作成するには、いくつかのオプションがあります。 このチュートリアルでは、最新の [AEMプロジェクトのアーキタイプ](https://github.com/adobe/aem-project-archetype) (Archetype)をチュートリアルコードの基礎として使用しました。 AEMの複数のバージョンをサポートするために、プロジェクトコードに変更が加えられました。 下位互換性 [に関する注意を確認してください](overview.md#compatibility)。

>[!CAUTION]
>
>ベストプラクティスとして、 **最新バージョンの** アーキタイプを使用して [](https://github.com/adobe/aem-project-archetype) 、実際の実装用の新しいプロジェクトを生成することができます。 AEMプロジェクトでは、アーキタイプの `aemVersion` プロパティを使用して1つのバージョンのAEMをターゲットする必要があります。

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 次のフォルダーとファイル構造は、ローカルファイルシステム上のMavenアーキタイプによって生成されたAEMプロジェクトを表しています。

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. The following properties were used when generating the AEM project from the [AEM Project archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | プロパティ | 値 |
   |-----------------|---------------------------------------|
   | aemVersion | クラウド |
   | appTitle | WKND SPAアンギュラ |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | 角 |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > プロパティに注目して `frontendModule=angular` ください。 これにより、AEM SPAエディタで使用するスターター [Angularコードベースを使用して、AEMプロジェクトのアーキタイプでプロジェクトをブートストラップする](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) 。

## プロジェクトの構築

次に、Mavenを使用して、プロジェクトコードをコンパイル、構築、およびAEMのローカルインスタンスにデプロイします。

1. AEMのインスタンスがローカルでポート4502で実行されているこ **とを確認します**。
2. コマンドラインターミナルから、Mavenがインストールされていることを確認します。

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 次のMaven下のコマンドを `aem-guides-wknd-spa` ディレクトリから実行し、プロジェクトを構築してAEMにデプロイします。

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   AEM [6.xを使用している場合](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   プロジェクトの複数のモジュールをコンパイルし、AEMにデプロイする必要があります。

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
    [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
    [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
    [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
    [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
    [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
    [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
    [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
    [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
    [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
   ```

   Mavenプロファイル ***autoInstallSinglePackage*** は、プロジェクトの個々のモジュールをコンパイルし、1つのパッケージをAEMインスタンスに展開します。 デフォルトでは、このパッケージは、ローカルのポート4502で実行され **ているAEMインスタンスにデプロイされ** 、 **admin:adminの資格情報が付与されます**。

4. Navigate to **[!UICONTROL Package Manager]** on your local AEM instance: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. との3つのパッケージが表示され `wknd-spa-angular.all`るはず `wknd-spa-angular.ui.apps` で `wknd-spa-angular.ui.content`す。

   ![WKND SPAパッケージ](./assets/create-project/package-manager.png)

   プロジェクトに必要なすべてのカスタムコードは、これらのパッケージにバンドルされ、AEMランタイムにインストールされます。

6. また、との複数のパッケージも表示さ `spa.project.core` れ `core.wcm.components`ます。 これらは、アーキタイプによって自動的に追加される依存関係です。 [AEMコアコンポーネントの詳細については、こちらを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)。

## 作成者コンテンツ

次に、アーキタイプによって生成されたスターターSPAを開き、コンテンツの一部を更新します。

1. Navigate to the **[!UICONTROL Sites]** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPAには、国、言語、ホームページを持つ基本的なサイト構造が含まれています。 この階層は、およびのアーキタイプのデフォルト値に基づ `language_country` き `isSingleCountryWebsite`ます。 これらの値は、プロジェクトの生成時に [使用可能なプロパティを更新することで上書きできます](https://github.com/adobe/aem-project-archetype#available-properties) 。

2. ページを選択し、メニューバーの **[!DNL us]** 「 **[!DNL en]** 編集 **[!DNL WKND SPA Angular Home Page]** 」ボタンをクリックして、// **** ページを開きます。

   ![サイトコンソール](./assets/create-project/open-home-page.png)

3. ページに **[!UICONTROL テキスト]** コンポーネントが既に追加されています。 このコンポーネントは、AEMの他のコンポーネントと同様に編集できます。

   ![テキストコンポーネントの更新](./assets/create-project/update-text-component.gif)

4. ペ追加ージの追加の **[!UICONTROL テキスト]** コンポーネント。

   オーサリングエクスペリエンスは、従来のAEM Sitesページのエクスペリエンスと似ています。 現在、使用できるコンポーネントの数に制限があります。 チュートリアルの過程でさらに追加されます。

## シングルページアプリのInspect

次に、ブラウザーの開発者ツールを使用して、これがシングルページアプリであることを確認します。

1. **[!UICONTROL ページエディターで]**、 **[!UICONTROL ページ情報]** (Page Information)メニューの「 **[!UICONTROL 表示は公開済み]**」をクリックします。

   ![「公開済みとして表示」ボタン](./assets/create-project/view-as-published.png)

   これにより、クエリパラメータを持つ新しいタブが開き、AEMエディタ `?wcmmode=disabled` が効果的にオフになります。 [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. ページのソースを表示し、テキストコンテンツ **[!DNL Hello World]** または他のコンテンツが見つからないことに注意します。 代わりに、次のようなHTMLが表示されます。

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` は、ページに読み込まれ、コンテンツのレンダリングを行うAngular SPAです。

   *コンテンツの提供元*

3. タブに戻る： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. ブラウザーの開発者ツールを開き、更新中のページのネットワークトラフィックを調べます。 XHR **要求の表示** :

   ![XHR要求](./assets/create-project/xhr-requests.png)

   http://localhost:4502/content/wknd-spa-angular/us/en.model.jsonにリクエストが送信され [ます](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 SPAをドライブするすべてのコンテンツがJSON形式で含まれます。

5. 新しいタブで、http://localhost:4502/content/wknd-spa-angular/us/en.model.jsonを開き [ます。](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   リクエストは、アプリを駆動するコンテンツモデルを `en.model.json` 表します。 JSON出力をInspectに送信します。 **[!UICONTROL Text]** コンポーネントを表すスニペットを見つけることができます。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   次の章では、AEM SPA Editorの使用感を基にして、AEMコンポーネントからSPAコンポーネントにJSONコンテンツがどのようにマッピングされるかを調べます。

   >[!NOTE]
   >
   > JSON出力の形式を自動的に設定するには、ブラウザー拡張機能をインストールすると便利です。

## バリデーターが{#congratulations}

AEM SPAエディタープロジェクトを初めて作成しました。

現時点では非常にシンプルですが、次の数章では、さらに機能が追加されます。

### 次の手順 {#next-steps}

[SPAの統合](integrate-spa.md) - SPAソースコードがAEMプロジェクトとどのように統合されているかを学び、SPAを迅速に開発するために利用可能なツールを理解します。
