---
title: プロジェクトを作成 | AEM SPA Editor と React の使用の手引き
description: AEM SPA Editor と統合された React アプリケーションの出発点として、Adobe Experience Manager(AEM)Maven プロジェクトを生成する方法を説明します。
sub-product: sites
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 3%

---

# プロジェクトを作成 {#spa-editor-project}

AEM SPA Editor と統合された React アプリケーションの出発点として、Adobe Experience Manager(AEM)Maven プロジェクトを生成する方法を説明します。

## 目的

1. AEMプロジェクトアーキタイプを使用して、SPA Editor が有効なプロジェクトを生成します。
2. スタータープロジェクトをAEMのローカルインスタンスにデプロイします。

## 作成する内容 {#what-build}

この章では、 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). AEMプロジェクトは、React SPAの非常にシンプルな出発点でブートストラップ処理されます。

**Maven プロジェクトとは** - [Apache Maven](https://maven.apache.org/) は、プロジェクトを構築するためのソフトウェア管理ツールです。 *すべてのAdobe Experience Manager* 実装では、Maven プロジェクトを使用して、AEM上にカスタムコードを作成、管理およびデプロイします。

**Maven アーキタイプとは** - A [Maven アーキタイプ](https://maven.apache.org/archetype/index.html) は、新しいプロジェクトを生成するためのテンプレートまたはパターンです。 AEMプロジェクトのアーキタイプを使用すると、カスタム名前空間を持つ新しいプロジェクトを生成し、ベストプラクティスに従ったプロジェクト構造を含めて、プロジェクトを大幅に高速化できます。

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment). Adobe Experience Managerの新しいインスタンスが、で始まることを確認します。 **作成者** モード、はローカルで実行されています。

## プロジェクトを作成する {#create}

>[!NOTE]
>
>このチュートリアルではバージョンを使用します **39** を使用します。 常に、 **latest** 新しいプロジェクトを生成するためのアーキタイプのバージョン。

1. コマンドラインターミナルを開き、次の Maven コマンドを入力します。

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=39 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > AEM 6.5.5 以降をターゲットにする場合は、 `aemVersion="cloud"` と `aemVersion="6.5.5"`. 6.4.8 以降をターゲットにする場合は、 `aemVersion="6.4.8"`.

   注意： `frontendModule=react` プロパティ。 これにより、AEMプロジェクトアーキタイプがスターターを使用してプロジェクトをブートストラップするようになります [React コードベース](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) をAEM SPA Editor で使用する場合。 次のようなプロパティ `appTitle`, `appId`, `artifactId`、および `groupId` を使用して、プロジェクトと目的を特定します。

   プロジェクトを設定するために使用できるプロパティの完全なリスト [ここにあります](https://github.com/adobe/aem-project-archetype#available-properties).

1. 次のフォルダーおよびファイル構造は、ローカルファイルシステム上の Maven アーキタイプによって生成されます。

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   各フォルダーは個々の Maven モジュールを表します。 このチュートリアルでは、主に `ui.frontend` モジュールとは、React アプリのことです。 個々のモジュールの詳細については、 [AEM Project Archetype ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## プロジェクトのデプロイとビルド

次に、Maven を使用して、プロジェクトコードをコンパイル、ビルドし、AEMのローカルインスタンスにデプロイします。

1. AEMのインスタンスがポート上でローカルで実行されていることを確認します **4502**.
1. コマンドラインから、 `aem-guides-wknd-spa.react` プロジェクトディレクトリ。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 次のコマンドを実行して、プロジェクト全体を構築し、AEMにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ビルドは約 1 分かかり、最後に次のメッセージが表示されます。

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

   Maven プロファイル `autoInstallSinglePackage` プロジェクトの個々のモジュールをコンパイルし、1 つのパッケージをAEMインスタンスにデプロイします。 デフォルトでは、このパッケージは、ポートでローカルに実行されているAEMインスタンスにデプロイされます **4502** そして `admin:admin`.

1. に移動します。 **パッケージマネージャー** ローカルAEMインスタンスで以下の手順を実行します。 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. 複数のパッケージの前に `aem-guides-wknd-spa.react`.

   ![WKND SPA Packages](assets/create-project/package-manager.png)

   *AEM Package Manager*

   プロジェクトに必要なすべてのカスタムコードは、これらのパッケージにバンドルされ、AEM環境にインストールされます。

## コンテンツを作成

次に、アーキタイプで生成されたスターターSPAを開き、コンテンツの一部を更新します。

1. 次に移動： **サイト** コンソール： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPAは、国、言語、ホームページを含む基本的なサイト構造を含んでいます。 この階層は、アーキタイプの `language_country` および `isSingleCountryWebsite`. これらの値は、 [使用可能なプロパティ](https://github.com/adobe/aem-project-archetype#available-properties) プロジェクトを生成する際に使用します。

2. を開きます。 **us** > **en** > **WKND SPA React ホームページ** ページを表示するには、ページを選択して **編集** ボタンをクリックします。

   ![サイトコンソール](./assets/create-project/open-home-page.png)

3. A **テキスト** コンポーネントは既にページに追加されています。 このコンポーネントは、AEMの他のコンポーネントと同様に編集できます。

   ![テキストコンポーネントの更新](./assets/create-project/update-text-component.gif)

4. 追加 **テキスト** コンポーネントをページに追加します。

   オーサリングエクスペリエンスは、従来のAEM Sitesページと似ています。 現在、使用できるコンポーネントの数は限られています。 チュートリアルの過程で、さらに追加されます。

## Inspect Single Page Application

次に、ブラウザーの開発者ツールを使用して、これがシングルページアプリケーションであることを確認します。

1. 内 **ページエディター**、 **ページ情報** ボタン > **公開済みとして表示**:

   ![「公開済みとして表示」ボタン](./assets/create-project/view-as-published.png)

   クエリパラメーターを含む新しいタブが開きます `?wcmmode=disabled` AEMエディターを効果的にオフにします。 [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. ページのソースを表示し、テキストコンテンツに注目してください **[!DNL Hello World]** または他のコンテンツが見つかりません。 代わりに、次のようなHTMLが表示されます。

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` は、ページに読み込まれ、コンテンツのレンダリングを担当する React SPAです。

   しかし、 *コンテンツの取得元*

3. 「 」タブに戻ります。 [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. ブラウザーの開発者ツールを開き、更新中にページのネットワークトラフィックを調べます。 次を表示： **XHR** リクエスト：

   ![XHR リクエスト](./assets/create-project/xhr-requests.png)

   ～するように要求すべきである。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). SPAを駆動する JSON 形式のすべてのコンテンツが含まれます。

5. 新しいタブで、を開きます。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   リクエスト `en.model.json` は、アプリケーションを駆動するコンテンツモデルを表します。 Inspect JSON 出力を参照し、 **[!UICONTROL テキスト]** コンポーネント。

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

   次の章では、この JSON コンテンツがAEMコンポーネントからSPAコンポーネントにどのようにマッピングされ、AEM SPAエディターのエクスペリエンスの基盤が形成されるかを調べます。

   >[!NOTE]
   >
   > JSON 出力を自動的にフォーマットするには、ブラウザー拡張機能をインストールすると役に立つ場合があります。

## おめでとうございます。 {#congratulations}

これで、最初のAEM SPA Editor プロジェクトが作成されました。

このSPAは非常に簡単です。 次の数章では、さらに機能が追加されます。

### 次の手順 {#next-steps}

[SPAの統合](integrate-spa.md) - SPAソースコードがAEMプロジェクトと統合される仕組みと、SPAを迅速に開発するために使用できるツールについて説明します。
