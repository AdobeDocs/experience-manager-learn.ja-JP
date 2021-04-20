---
title: SPA Editorプロジェクト | AEM SPA EditorとReactの使い始めに
description: AEM SPAエディタと統合されたReactアプリケーションの起点として、Adobe Experience Manager(AEM) Mavenプロジェクトを使用する方法を説明します。
sub-product: サイト
feature: SPA Editor, AEM Project Archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 4%

---


# SPAエディタプロジェクト{#spa-editor-project}

AEM SPAエディタと統合されたReactアプリケーションの起点として、Adobe Experience Manager(AEM) Mavenプロジェクトを使用する方法を説明します。

## 目的

1. Mavenアーキタイプから構築された新しいAEM SPAエディタプロジェクトの構造を理解します。
2. スタータープロジェクトをAEMのローカルインスタンスにデプロイします。

## 作成する内容

この章では、[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)に基づいて、新しいAEMプロジェクトを導入します。 AEMプロジェクトは、React SPAの非常に簡単な開始点でブートストラップされます。 本章で使用するプロジェクトは、WKND SPAの実装の基盤となり、今後の章で構築される予定です。

![WKND SPA React Starterプロジェクト](./assets/create-project/wknd-spa-react.png)

*WKND SPAのサイト階層の開始。*

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 **author**&#x200B;モードで開始したAdobe Experience Managerの新しいインスタンスがローカルで実行されていることを確認します。

## プロジェクトの取得

AEM用のMavenマルチモジュールプロジェクトを作成するには、いくつかのオプションがあります。 このチュートリアルでは、最新の[AEMプロジェクトのアーキタイプ](https://github.com/adobe/aem-project-archetype)をチュートリアルコードの基礎として使用しました。 AEMの複数のバージョンをサポートするために、プロジェクトコードに変更が加えられました。 [下位互換性](overview.md#compatibility)に関する注意を確認してください。

>[!CAUTION]
>
> [アーキタイプ](https://github.com/adobe/aem-project-archetype)の&#x200B;**最新バージョン**&#x200B;を使用して、現実世界での実装用の新しいプロジェクトを生成することがベストプラクティスです。 AEMプロジェクトでは、アーキタイプの`aemVersion`プロパティを使用してAEMの単一バージョンをターゲットする必要があります。

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
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

3. [AEMプロジェクトのアーキタイプ](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)からAEMプロジェクトを生成する際に、次のプロパティが使用されました。

   | プロパティ | 値 |
   |-----------------|-------------------------------------|
   | aemVersion | クラウド |
   | appTitle | WKND SPA React |
   | appId | wknd-spa-react |
   | groupId | com.adobe.aem.guides |
   | frontendModule | 反応する |
   | package | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > `frontendModule=react`プロパティに注目してください。 これにより、AEMプロジェクトのアーキタイプは、プロジェクトをスターター[リアクトコードベース](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)でブートストラップし、AEM SPAエディタで使用するように指示されます。

## プロジェクトの構築

次に、Mavenを使用して、プロジェクトコードをコンパイル、構築、およびAEMのローカルインスタンスにデプロイします。

1. AEMのインスタンスがローカルでポート&#x200B;**4502**&#x200B;上で実行されていることを確認します。
2. コマンドラインターミナルから、Mavenがインストールされていることを確認します。

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. `aem-guides-wknd-spa`ディレクトリから下のMavenコマンドを実行し、プロジェクトを構築してAEMにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)を使用する場合：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   プロジェクトの複数のモジュールをコンパイルし、AEMにデプロイする必要があります。

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   Mavenプロファイル&#x200B;***autoInstallSinglePackage***&#x200B;は、プロジェクトの個々のモジュールをコンパイルし、1つのパッケージをAEMインスタンスに展開します。 デフォルトでは、このパッケージは、ローカルのポート&#x200B;**4502**&#x200B;を実行し、**admin:admin**&#x200B;の資格情報を持つAEMインスタンスに展開されます。

4. ローカルAEMインスタンスで&#x200B;**[!UICONTROL Package Manager]**&#x200B;に移動します。[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. `wknd-spa-react.all`, `wknd-spa-react.ui.apps`, `wknd-spa-react.ui.content`の3つのパッケージが見えるはずです。

   ![WKND SPAパッケージ](./assets/create-project/package-manager.png)

   *AEM Package Manager*

   プロジェクトに必要なすべてのカスタムコードは、これらのパッケージにバンドルされ、AEMランタイムにインストールされます。

6. また、`spa.project.core`と`core.wcm.components`のパッケージもいくつか見てください。 これらの依存関係は、アーキタイプによって自動的に含まれます。 [AEMコアコンポーネントの詳細は、](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/introduction.html)を参照してください。

   `spa.project.core` は、SPAエディターで必要なJSONモデルAPIの生成に必要な依存関係です。

## 作成者コンテンツ

次に、アーキタイプで生成されたスターターSPAを開き、コンテンツの一部を更新します。

1. **[!UICONTROL サイト]**&#x200B;コンソールに移動します。[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPAは、国、言語、ホームページを持つ基本的なサイト構造を備えています。 この階層は、アーキタイプの`language_country`および`isSingleCountryWebsite`のデフォルト値に基づいています。 これらの値は、プロジェクトの生成時に[使用可能なプロパティ](https://github.com/adobe/aem-project-archetype#available-properties)を更新することで上書きできます。

2. **[!DNL us]**/**[!DNL en]**/**[!DNL WKND SPA React Home Page]**&#x200B;ページを開きます。ページを選択し、メニューバーの「**[!UICONTROL 編集]**」ボタンをクリックします。

   ![サイトコンソール](./assets/create-project/open-home-page.png)

3. **[!UICONTROL テキスト]**&#x200B;コンポーネントは既にページに追加されています。 このコンポーネントは、AEMの他のコンポーネントと同様に編集できます。

   ![テキストコンポーネントの更新](./assets/create-project/update-text-component.gif)

4. ページに追加追加の&#x200B;**[!UICONTROL テキスト]**&#x200B;コンポーネント。

   オーサリングエクスペリエンスは、従来のAEM Sitesページのエクスペリエンスと似ています。 現在、使用できるコンポーネントの数に制限があります。 チュートリアルの過程でさらに追加されます。

## シングルページアプリのInspect

次に、ブラウザーの開発者ツールを使用して、これがシングルページアプリであることを確認します。

1. **[!UICONTROL ページエディター]**&#x200B;で、**[!UICONTROL ページ表示]**&#x200B;ボタン/**[!UICONTROL 発行済みの情報をクリックします。]**

   ![「公開済みとして表示」ボタン](./assets/create-project/view-as-published.png)

   これにより、クエリパラメータ`?wcmmode=disabled`を持つ新しいタブが開き、AEMエディタが効果的にオフになります。[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. ページのソースを表示し、テキストコンテンツ&#x200B;**[!DNL Hello World]**&#x200B;または他のコンテンツが見つからないことに注意してください。 代わりに、次のようなHTMLが表示されます。

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` は、ページに読み込まれ、コンテンツのレンダリングを行うReact SPAです。

   ただし、*コンテンツの由来は？*

3. タブに戻る：[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. ブラウザーの開発者ツールを開き、更新中のページのネットワークトラフィックを調べます。 **XHR**&#x200B;リクエストの表示:

   ![XHR要求](./assets/create-project/xhr-requests.png)

   [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)にリクエストが必要です。 SPAを駆動するJSON形式のすべてのコンテンツが含まれます。

5. 新しいタブで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を開きます。

   リクエスト`en.model.json`は、アプリケーションを駆動するコンテンツモデルを表します。 JSON出力をInspectに送信し、**[!UICONTROL テキスト]**&#x200B;コンポーネントを表すスニペットを見つけることができるはずです。

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

   次の章では、AEM SPAエディターの使用感を基にして、このJSONコンテンツがAEMコンポーネントからSPAコンポーネントにどのようにマッピングされるかを調べます。

   >[!NOTE]
   >
   > JSON出力の形式を自動的に設定するには、ブラウザー拡張機能をインストールすると便利です。

## これで完了です! {#congratulations}

AEM SPA Editorプロジェクトを初めて作成しました。

SPAは非常に単純です。 次の数章では、さらに機能が追加されます。

### 次の手順 {#next-steps}

[SPAの統合](integrate-spa.md) - SPAソースコードがAEMプロジェクトと統合されている方法を学び、SPAを迅速に開発するためのツールを理解します。
