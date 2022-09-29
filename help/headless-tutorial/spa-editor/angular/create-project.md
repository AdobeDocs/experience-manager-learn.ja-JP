---
title: SPA Editor Project | AEM SPA Editor とAngularの概要
description: AEM SPA Editor と統合されたAngularアプリケーションの出発点として、Adobe Experience Manager(AEM)Maven プロジェクトを使用する方法を説明します。
sub-product: sites
feature: SPA Editor, AEM Project Archetype
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 4%

---

# SPA Editor Project {#create-project}

AEM SPA Editor と統合されたAngularアプリケーションの出発点として、Adobe Experience Manager(AEM)Maven プロジェクトを使用する方法を説明します。

## 目的

1. Maven アーキタイプから構築された新しいAEM SPA Editor プロジェクトの構造を理解します。
2. スタータープロジェクトをAEMのローカルインスタンスにデプロイします。

## 作成する内容

この章では、 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). AEMプロジェクトは、AngularSPAの非常にシンプルな出発点でブートストラップ処理されます。 この章で使用されるプロジェクトは、WKND SPAの実装の基礎となり、将来の章に基づいて構築されます。

![WKND SPAAngularスタータープロジェクト](./assets/create-project/what-you-will-build.png)

*クラシックな Hello World メッセージです。*

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment). Adobe Experience Managerの新しいインスタンスが、で始まることを確認します。 **作成者** モード、はローカルで実行されています。

## プロジェクトを取得

AEM用の Maven マルチモジュールプロジェクトを作成するには、いくつかのオプションがあります。 このチュートリアルでは、最新の [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) を参照してください。 複数のバージョンのAEMをサポートするために、プロジェクトコードが変更されました。 確認してください [後方互換性に関する注意事項](overview.md#compatibility).

>[!CAUTION]
>
>これは、 **latest** のバージョン [アーキタイプ](https://github.com/adobe/aem-project-archetype) 実際の実装に新しいプロジェクトを生成する。 AEMプロジェクトでは、 `aemVersion` プロパティに含まれます。

1. このチュートリアルの開始点を Git からダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 次のフォルダーとファイル構造は、ローカルファイルシステム上の Maven アーキタイプによって生成されたAEMプロジェクトを表しています。

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

3. 次のプロパティが、 [AEMプロジェクトアーキタイプ](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | プロパティ | 値 |
   |-----------------|---------------------------------------|
   | aemVersion | クラウド |
   | appTitle | WKNDSPAAngular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 注意： `frontendModule=angular` プロパティ。 これにより、AEMプロジェクトアーキタイプがスターターを使用してプロジェクトをブートストラップするようになります [Angularコードベース](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) をAEM SPA Editor で使用する場合。

## プロジェクトの構築

次に、Maven を使用して、プロジェクトコードをコンパイル、ビルドし、AEMのローカルインスタンスにデプロイします。

1. AEMのインスタンスがポート上でローカルで実行されていることを確認します **4502**.
2. コマンドラインターミナルから、Maven がインストールされていることを確認します。

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 次の Maven コマンドを `aem-guides-wknd-spa` プロジェクトを構築してAEMにデプロイするディレクトリ：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   を使用する場合 [AEM 6.x](overview.md#compatibility):

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

   Maven プロファイル ***autoInstallSinglePackage*** プロジェクトの個々のモジュールをコンパイルし、1 つのパッケージをAEMインスタンスにデプロイします。 デフォルトでは、このパッケージは、ポートでローカルに実行されているAEMインスタンスにデプロイされます **4502** そして **admin:admin**.

4. に移動します。 **[!UICONTROL パッケージマネージャー]** ローカルAEMインスタンスで以下の手順を実行します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 次の 3 つのパッケージが表示されます。 `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` および `wknd-spa-angular.ui.content`.

   ![WKND SPA Packages](./assets/create-project/package-manager.png)

   プロジェクトに必要なすべてのカスタムコードは、これらのパッケージにバンドルされ、AEMランタイムにインストールされます。

6. また、 `spa.project.core` および `core.wcm.components`. これらは、アーキタイプによって自動的に含まれる依存関係です。 詳細情報： [AEMコアコンポーネントは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja).

## コンテンツを作成

次に、アーキタイプで生成されたスターターSPAを開き、コンテンツの一部を更新します。

1. 次に移動： **[!UICONTROL サイト]** コンソール： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPAは、国、言語、ホームページを含む基本的なサイト構造を含んでいます。 この階層は、アーキタイプの `language_country` および `isSingleCountryWebsite`. これらの値は、 [使用可能なプロパティ](https://github.com/adobe/aem-project-archetype#available-properties) プロジェクトを生成する際に使用します。

2. を開きます。 **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** ページを表示するには、ページを選択して **[!UICONTROL 編集]** ボタンをクリックします。

   ![サイトコンソール](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL テキスト]** コンポーネントは既にページに追加されています。 このコンポーネントは、AEMの他のコンポーネントと同様に編集できます。

   ![テキストコンポーネントの更新](./assets/create-project/update-text-component.gif)

4. 追加 **[!UICONTROL テキスト]** コンポーネントをページに追加します。

   オーサリングエクスペリエンスは、従来のAEM Sitesページと似ています。 現在、使用できるコンポーネントの数は限られています。 チュートリアルの過程で、さらに追加されます。

## Inspect Single Page Application

次に、ブラウザーの開発者ツールを使用して、これがシングルページアプリケーションであることを確認します。

1. 内 **[!UICONTROL ページエディター]**、 **[!UICONTROL ページ情報]** メニュー/ **[!UICONTROL 公開済みとして表示]**:

   ![「公開済みとして表示」ボタン](./assets/create-project/view-as-published.png)

   クエリパラメーターを含む新しいタブが開きます `?wcmmode=disabled` AEMエディターを効果的にオフにします。 [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. ページのソースを表示し、テキストコンテンツに注目してください **[!DNL Hello World]** または他のコンテンツが見つかりません。 代わりに、次のようなHTMLが表示されます。

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

   `clientlib-angular.min.js` は、ページに読み込まれ、コンテンツのレンダリングを担当するAngularSPAです。

   *コンテンツの取得元*

3. 「 」タブに戻ります。 [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. ブラウザーの開発者ツールを開き、更新中にページのネットワークトラフィックを調べます。 次を表示： **XHR** リクエスト：

   ![XHR リクエスト](./assets/create-project/xhr-requests.png)

   ～するように要求すべきである。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). SPAを駆動する JSON 形式のすべてのコンテンツが含まれます。

5. 新しいタブで、を開きます。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   リクエスト `en.model.json` は、アプリケーションを駆動するコンテンツモデルを表します。 Inspect JSON 出力を参照し、 **[!UICONTROL テキスト]** コンポーネント。

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

   次の章では、JSON コンテンツがAEMコンポーネントからSPAコンポーネントにマッピングされ、AEM SPAエディターのエクスペリエンスの基盤が形成される方法を調べます。

   >[!NOTE]
   >
   > JSON 出力を自動的にフォーマットするには、ブラウザー拡張機能をインストールすると役に立つ場合があります。

## おめでとうございます。 {#congratulations}

これで、最初のAEM SPA Editor プロジェクトが作成されました。

現在は非常にシンプルですが、今後の数章では、さらに機能が追加されます。

### 次の手順 {#next-steps}

[SPAの統合](integrate-spa.md) - SPAソースコードがAEMプロジェクトと統合される仕組みと、SPAを迅速に開発するために使用できるツールについて説明します。
