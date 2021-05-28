---
title: プロジェクトを作成 | AEM SPA EditorとReactの概要
description: AEM SPA Editorと統合されたReactアプリケーションの出発点として、Adobe Experience Manager(AEM)Mavenプロジェクトを生成する方法を説明します。
sub-product: サイト
feature: SPAエディター、AEMプロジェクトアーキタイプ
version: cloud-service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 3%

---


# プロジェクトを作成 {#spa-editor-project}

AEM SPA Editorと統合されたReactアプリケーションの出発点として、Adobe Experience Manager(AEM)Mavenプロジェクトを生成する方法を説明します。

## 目的

1. AEMプロジェクトアーキタイプを使用して、SPA Editorが有効なプロジェクトを生成します。
2. スタータープロジェクトをAEMのローカルインスタンスにデプロイします。

## 作成する内容 {#what-build}

この章では、[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)に基づいて、新しいAEMプロジェクトが生成されます。 AEMプロジェクトは、React SPAの非常にシンプルな出発点でブートストラップ処理されます。

**Mavenプロジェクトとは** -  [Apache Mavenis](https://maven.apache.org/) は、プロジェクトを構築するソフトウェア管理ツールです。*すべてのAdobe Experience Manager実* 装では、Mavenプロジェクトを使用して、AEM上にカスタムコードを作成、管理およびデプロイします。

**Mavenアーキタイプとは** - Mavenアーキタ [イ](https://maven.apache.org/archetype/index.html) プは、新しいプロジェクトを生成するためのテンプレートまたはパターンです。AEMプロジェクトのアーキタイプを使用すると、カスタム名前空間を持つ新しいプロジェクトを生成し、ベストプラクティスに従うプロジェクト構造を含めて、プロジェクトを大幅に高速化できます。

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 **author**&#x200B;モードで開始した、Adobe Experience Managerの新しいインスタンスがローカルで実行されていることを確認します。

## プロジェクトの作成 {#create}

>[!NOTE]
>
>このチュートリアルでは、アーキタイプのバージョン&#x200B;**27**&#x200B;を使用します。 新しいプロジェクトを生成する場合は、常にアーキタイプの&#x200B;**最新の**&#x200B;バージョンを使用することをお勧めします。

1. コマンドラインターミナルを開き、次のMavenコマンドを入力します。

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.5以降をターゲットにする場合は、 `aemVersion="cloud"`を`aemVersion="6.5.5"`に置き換えます。 6.4.8以降をターゲットにする場合は、`aemVersion="6.4.8"`を使用します。

   `frontendModule=react`プロパティに注意してください。 これにより、AEM SPA Editorで使用するスターター[Reactコードベース](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)でプロジェクトをブートストラップするよう、AEMプロジェクトアーキタイプに指示します。 `appTitle`、`appId`、`artifactId`、`groupId`などのプロパティを使用して、プロジェクトと目的を特定します。

   プロジェクト[の設定に使用できるプロパティの完全なリストは、](https://github.com/adobe/aem-project-archetype#available-properties)にあります。

1. 次のフォルダーとファイル構造は、ローカルファイルシステム上のMavenアーキタイプによって生成されます。

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
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

   各フォルダーは個々のMavenモジュールを表します。 このチュートリアルでは、主に`ui.frontend`モジュール（Reactアプリ）を使用します。 個々のモジュールに関する詳細は、[AEMプロジェクトアーキタイプのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)を参照してください。

## プロジェクトのデプロイとビルド

次に、Mavenを使用して、プロジェクトコードをコンパイル、ビルド、AEMのローカルインスタンスにデプロイします。

1. AEMのインスタンスがポート&#x200B;**4502**&#x200B;上でローカルに実行されていることを確認します。
1. コマンドラインから`aem-guides-wknd-spa.react`プロジェクトディレクトリに移動します。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 次のコマンドを実行して、プロジェクト全体を構築し、AEMにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ビルドには約1分かかり、最後に次のメッセージが表示されます。

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Mavenプロファイル`autoInstallSinglePackage`は、プロジェクトの個々のモジュールをコンパイルし、AEMインスタンスに1つのパッケージをデプロイします。 デフォルトでは、このパッケージは、ポート&#x200B;**4502**&#x200B;でローカルに実行され、資格情報`admin:admin`を持つAEMインスタンスにデプロイされます。

1. ローカルのAEMインスタンスで&#x200B;**パッケージマネージャー**&#x200B;に移動します。[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. `aem-guides-wknd-spa.react`というプレフィックスが付いた複数のパッケージが表示されます。

   ![WKND SPA Packages](assets/create-project/package-manager.png)

   *AEM Package Manager*

   プロジェクトに必要なすべてのカスタムコードは、これらのパッケージにバンドルされ、AEM環境にインストールされます。

## 作成者コンテンツ

次に、アーキタイプで生成されたスターターSPAを開き、コンテンツの一部を更新します。

1. **サイト**&#x200B;コンソールに移動します。[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPAは、国、言語、ホームページを備えた基本的なサイト構造を含んでいます。 この階層は、`language_country`と`isSingleCountryWebsite`のアーキタイプのデフォルト値に基づいています。 これらの値は、プロジェクトの生成時に[使用可能なプロパティ](https://github.com/adobe/aem-project-archetype#available-properties)を更新することで上書きできます。

2. ページを選択し、メニューバーの「**編集**」ボタンをクリックして、**us** > **en** > **WKND SPA Reactホームページ**&#x200B;を開きます。

   ![サイトコンソール](./assets/create-project/open-home-page.png)

3. **テキスト**&#x200B;コンポーネントが既にページに追加されています。 このコンポーネントは、AEMの他のコンポーネントと同様に編集できます。

   ![テキストコンポーネントの更新](./assets/create-project/update-text-component.gif)

4. 追加の&#x200B;**テキスト**&#x200B;コンポーネントをページに追加します。

   オーサリングエクスペリエンスは、従来のAEM Sitesページと似ています。 現在使用できるコンポーネントの数は限られています。 このチュートリアルでは、さらに詳しく説明します。

## Inspect the Single Page Application

次に、ブラウザーの開発者ツールを使用して、これがシングルページアプリケーションであることを確認します。

1. **ページエディター**&#x200B;で、**ページ情報**&#x200B;ボタン/**公開済みとして表示**&#x200B;をクリックします。

   ![「公開済みとして表示」ボタン](./assets/create-project/view-as-published.png)

   これにより、クエリパラメーター`?wcmmode=disabled`を持つ新しいタブが開き、AEMエディターが効果的にオフになります。[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. ページのソースを表示し、テキストコンテンツ&#x200B;**[!DNL Hello World]**&#x200B;や他のコンテンツが見つからないことに注意してください。 代わりに、次のようなHTMLが表示されます。

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` は、ページに読み込まれ、コンテンツのレンダリングを担当するReact SPAです。

   ただし、*コンテンツの送信元は？*

3. 「 」タブに戻ります。[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. ブラウザーの開発者ツールを開き、更新中にページのネットワークトラフィックを調べます。 **XHR**&#x200B;リクエストを表示します。

   ![XHR要求](./assets/create-project/xhr-requests.png)

   [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)へのリクエストが必要です。 SPAを駆動するJSON形式のすべてのコンテンツが含まれます。

5. 新しいタブで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を開きます。

   リクエスト`en.model.json`は、アプリケーションを駆動するコンテンツモデルを表します。 JSON出力をInspectに送信すると、**[!UICONTROL テキスト]**&#x200B;コンポーネントを表すスニペットが表示されます。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   次の章では、このJSONコンテンツがAEMコンポーネントからSPAコンポーネントにマッピングされ、AEM SPA Editorのエクスペリエンスの基盤となる方法を調べます。

   >[!NOTE]
   >
   > JSON出力を自動的にフォーマットするには、ブラウザー拡張機能をインストールすると便利です。

## バリデーターが {#congratulations}

これで、最初のAEM SPA Editorプロジェクトが作成されました。

このSPAは非常に単純だ。 次の数章では、さらに機能が追加されます。

### 次の手順 {#next-steps}

[SPAの統合](integrate-spa.md)  - SPAソースコードをAEMプロジェクトと統合する方法と、SPAを迅速に開発するために使用できるツールについて説明します。
