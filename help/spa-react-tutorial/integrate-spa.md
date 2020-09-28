---
title: SPAの統合 | AEM SPAエディターとReactの使い始めに
description: Reactで記述されたシングルページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 WebPack開発サーバーなど、最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対してSPAを迅速に開発する方法を説明します。
sub-product: サイト
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 4%

---


# SPAの統合 {#integrate-spa}

Reactで記述されたシングルページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 WebPack開発サーバーなど、最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対してSPAを迅速に開発する方法を説明します。

## 目的

1. SPAプロジェクトがAEMとクライアント側ライブラリを統合する方法を理解します。
2. WebPack開発サーバーを使用して、専用のフロントエンド開発を行う方法を説明します。
3. AEM JSONモデルAPIに対する開発に **プロキシ** および静的 **モック** ファイルの使用を調査します。

## 作成する内容

この章では、SPAに単純な `Header` コンポーネントを追加します。 この静的コンポーネントを構築する過程で、AEM SPA開発に対するいくつかのアプローチが使用されます。 `Header`

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的コンポー`Header`ネントが追加されます*

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/integrate-spa-solution`ます。

## Integration approach {#integration-approach}

2つのモジュールがAEMプロジェクトの一部として作成されました。 `ui.apps` と `ui.frontend`。

この `ui.frontend` モジュールは、すべてのSPAソースコードを含む [webpack](https://webpack.js.org/) プロジェクトです。 SPAの開発およびテストの大部分は、WebPackプロジェクトで行われます。 実稼働版ビルドがトリガーされると、SPAはWebPackを使用して構築およびコンパイルされます。 コンパイルされたアーティファクト（CSSとJavaScript）が `ui.apps` モジュールにコピーされ、モジュールがAEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の高レベルな描写。*

フロントエンドビルドの詳細については、こちらを [参照してください](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

## InspectSPA統合 {#inspect-spa-integration}

次に、 `ui.frontend` モジュールを調べ、AEM [Projectアーキタイプによって自動生成されたSPAを確認します](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

1. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 このチュートリアルでは、 [Visual StudioコードIDEを使用します](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPAプロジェクト](./assets/integrate-spa/vscode-ide-openproject.png)

2. フォルダを展開し、 `ui.frontend` フォルダを調べます。 Open the file `ui.frontend/package.json`

3. の下に、 `dependencies``react` `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   こ `ui.frontend` れは、 [短くは「Create React App](https://create-react-app.dev/) 」または「CRA」に基づくReactアプリです。 バージョンは、使用されるCRAのバージョンを示し `react-scripts` ます。

4. また、次の3つの依存関係がプリフィックス付けされてい `@adobe`ます。

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上記のモジュールは、 [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) 、およびSPAコンポーネントをAEMコンポーネントにマッピングできる機能を提供します。

5. ファイルには、次のようにいくつかの定義済みの `package.json` ファイルがあ `scripts` ります。

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   これらは、Create React Appで作成された標準的なビルドスクリプト [で](https://create-react-app.dev/docs/available-scripts) 、

   唯一の違いは、スクリプ `&& clientlib``build` トへの追加です。 この追加命令は、ビルド中に、コンパイルされたSPAをクライアント側ライブラリとして `ui.apps` モジュールにコピーする役割を持ちます。

   npmモジュール [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) を使用して、この作業を容易に行うことができます。

6. ファイルをInspect `ui.frontend/clientlib.config.js`。 この設定ファイルは、 [aem-clientlib-generatorによって](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 、クライアントライブラリの生成方法を決定するために使用されます。

7. ファイルをInspect `ui.frontend/pom.xml`。 このファイルは、 `ui.frontend` フォルダーを [Mavenモジュールに変換します](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 この `pom.xml` ファイルは、Mavenの構築中にSPAの [テストと](https://github.com/eirslett/frontend-maven-plugin) 構築を行うために、フ **ロントエンドMavenプラグインを使用するように更新されました****** 。

8. Inspectのファイル `index.js` の場所 `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` は、SPAのエントリポイントです。 `ModelManager` は、AEM SPAエディターJS SDKによって提供されます。 アプリケーションに対して呼び出し、 `pageModel` （JSONコンテンツ）を挿入します。

## ヘッダー追加コンポーネント {#header-component}

次に、SPAに新しいコンポーネントを追加し、変更をローカルのAEMインスタンスにデプロイします。

1. モジュールで、 `ui.frontend` の下に、という名前の新しいフォルダーを `ui.frontend/src/components` 作成し `Header`ます。
2. Create a file named `Header.js` beneath the `Header` folder.

   ![ヘッダーフォルダーとファイル](assets/integrate-spa/header-folder-js.png)

3. `Header.js` に以下を入力します。

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   上記は、静的テキスト文字列を出力する標準的なReactコンポーネントです。

4. Open the file `ui.frontend/src/App.js`. これは、アプリケーションのエントリポイントです。
5. 静的なを含めるには、次 `App.js` の更新を行いま `Header`す。

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

6. 新しいターミナルを開き、 `ui.frontend` フォルダーに移動して、次の `npm run build` コマンドを実行します。

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

7. `ui.apps` フォルダーに移動し、の下 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` に、コンパイル済みのSPAファイルがフォルダーからコピーされたことが確認できます`ui.frontend/build` 。

   ![ui.appsで生成されたクライアントライブラリ](./assets/integrate-spa/compiled-spa-uiapps.png)

8. ターミナルに戻り、 `ui.apps` フォルダーに移動します。 次のMavenコマンドを実行します。

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   これにより、AEMのローカルで実行されている `ui.apps` インスタンスにパッケージが展開されます。

9. ブラウザーのタブを開き、http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 これで、SPAにコンポー `Header` ネントの内容が表示されます。

   ![最初のヘッダー実装](./assets/integrate-spa/initial-header-implementation.png)

   手順6 ～ 8は、プロジェクトのルートからMavenビルドをトリガーするとき(つまり、 `mvn clean install -PautoInstallSinglePackage`)に自動的に実行されます。 これで、SPAとAEMクライアント側ライブラリの統合の基本を理解する必要があります。 静的コンポーネントの下にあるAEMでは、コンポー `Text` ネントを編集および追加できることに注意してく `Header` ださい。

## Webpack Dev Server - JSON APIのプロキシ {#proxy-json}

前の練習で見たように、クライアントライブラリを構築してAEMのローカルインスタンスに同期する場合は、数分かかります。 これは、最終テストでは許容されますが、SPA開発の大部分には適していません。

SPAを迅速に開発するには、 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) (webpack-dev-server)を使用します。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、実行中のAEMインスタンスのJSONコンテンツが、開発サーバーに **プロキシ** されます。

1. IDEに戻り、ファイルを開き `ui.frontend/package.json`ます。

   次のような行を探します。

   ```json
   "proxy": "http://localhost:4502",
   ```

   「リアクトアプリを [作成](https://create-react-app.dev/docs/proxying-api-requests-in-development) 」は、APIリクエストをプロキシするための簡単なメカニズムです。 不明なリクエストはすべて、ローカルのAEM quickstart `localhost:4502`経由でプロキシされます。

2. Open a terminal window and navigate to the `ui.frontend` folder. Run the command `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

3. 新しいブラウザータブを開き（まだ開いていない場合）、http://localhost:3000/content/wknd-spa-react/us/en/home.htmlに移動し [ます](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack Devサーバー — プロキシjson](./assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、有効になっているオーサリング機能が1つもない場合は表示されます。

   >[!NOTE]
   >
   > AEMのセキュリティ要件により、同じブラウザーで、別のタブでローカルAEMインスタンス(http://localhost:4502)にログインする必要があります。

4. IDEに戻り、に示す新しいフォルダを作成 `media` し `ui.frontend/src/media`ます。
5. 次のWKNDロゴをダウンロードし、 `media` フォルダに追加します。

   ![WKNDロゴ](./assets/integrate-spa/wknd-logo-dk.png)

6. 次の場所 `Header.js` で開き、ロゴ `ui.frontend/src/components/Header/Header.js` を読み込みます。

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. ヘッダーにロゴを含め `Header.js` るには、次のように更新します。

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   に変更を保存し `Header.js`ます。

8. ブラウザー(http://localhost:3000/content/wknd-spa-react/us/en/home.html)に戻り [ます](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたロゴ](./assets/integrate-spa/added-logo-localhost.png)

   アドビがコンテンツをプロキシしているので、AEMでコンテンツを更新し続けて、 **webpack-dev-server**&#x200B;に反映されていることを確認できます。

9. ターミナルで、WebPack Devサーバー `ctrl+c` を停止します。

## Webpack Dev Server — モックJSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的なJSONファイルを使用してJSONモデルとして機能させることです。 JSONを「モッキング」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者がJSONモデルを更新して機能をテストし、変更をJSON APIに導き、後でバックエンド開発者が実装することになります。

The initial set up of the mock JSON does **require a local AEM instance**.

1. ブラウザーで、http://localhost:4502/content/wknd-spa-react/us/en.model.jsonに移動し [ます](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

2. IDEに戻り、という名前の新しいフォルダ `ui.frontend/public` に移動して追加 `mock-content`します。
3. Create a new file named `mock.model.json` beneath `ui.frontend/public/mock-content`. 手 **順1のJSON出力をここに貼り付けます** 。

   ![モックモデルJSONファイル](./assets/integrate-spa/mock-model-json-created.png)

4. Open the file `index.html` at `ui.frontend/public/index.html`. 変数を指すようにAEMページモデルのメタデータプロパティを更新し `%REACT_APP_PAGE_MODEL_PATH%`ます。

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   の値に変数を使用すると、プロキシとモックJSONモデルを簡単に切り替えるこ `cq:pagemodel_root_url` とができます。

5. ファイルを開き、次の更新 `ui.frontend/.env.development` を行って、の以前の値をコメントアウトし `REACT_APP_PAGE_MODEL_PATH`ます。

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 現在実行中の場合は、 **webpack-dev-server**. ターミナルから **webpack-dev-server** を開始します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   http://localhost:3000/content/wknd-spa-react/us/en/home.html [に移動すると](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 、プ **ロキシ** jsonで使用されているのと同じコンテンツを持つSPAが表示されます。

7. 以前に作成した `mock.model.json` ファイルに少し変更を加えます。 更新されたコンテンツは、 **webpack-dev-serverに直ちに反映されます**。

   ![モデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

JSONモデルを操作し、実稼働中のSPAへの影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行して行うこともできます。

次のように、 `env.development` ファイル内のエントリを切り替えて、JSONコンテンツを使用する場所を切り替えることができます。

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Sassで追加のスタイル

React では、各コンポーネントを自己完結型のモジュールにすることが推奨されます。一般的には、コンポーネント間で同じCSSクラス名を再使用しないようにすることをお勧めします。これにより、プリプロセッサーはあまり強力ではありません。 This project will use [Sass](https://sass-lang.com/) for a few useful features like variables. This project will also loosely follow [SUIT CSS naming conventions](https://github.com/suitcss/suit/blob/master/doc/components.md). SUIT は BEM（Block Element Modifier）記法の 1 つで、一貫性のある CSS ルールの作成に使用されます。

1. ターミナルウィンドウを開き、 **webpack-dev-server** （起動した場合）を停止します。 フォルダ内から次のコマンドを入力して、Sassを `ui.frontend` インストールします [](https://create-react-app.dev/docs/adding-a-sass-stylesheet)。

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   ピア依存関係 `sass` としてインストール：

   ```shell
   $ npm install sass --save
   ```

2. ブラウザー `normalize-scss` 全体でスタイルを正規化するには、をインストールします。

   ```shell
   $ npm install normalize-scss
   ```

3. 更新されたスタイルをリアルタイムで確認できるように **webpack-dev-server** 開始します。

   ```shell
   $ npm start
   ```

   JSONモデルAPIを処理するには、モックのプロキシ方法のいずれかを使用します。

4. IDEに戻り、次にという名前の新しいフォルダ `ui.frontend/src` を作成し `styles`ます。
5. 名前の下に新しいファイルを作成し、次の変数 `ui.frontend/src/styles``_variables.scss` を使用して入力します。

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. ファイルの拡張子の名前をに変更 `index.css` し `ui.frontend/src/index.css` ま **`index.scss`**&#x200B;す。 内容を次の内容に置き換えます。

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. 名前を変更 `ui.frontend/src/index.js` して、次の名前を含めるように更新し `index.scss`ます。

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Create a new file named `Header.scss` beneath `ui.frontend/src/components/Header`. ファイルに以下のように入力します。

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. 更新 `Header.scss` によって含める `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. ブラウザに戻り、 **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![スタイル設定ヘッダー — webpack devサーバー](./assets/integrate-spa/styled-header.png)

   コンポーネントに追加された更新済みのスタイルが表示され `Header` ます。

## SPA更新をAEMに展開する

に対する変更は、現在 `Header` は **webpack-dev-serverを通してのみ表示されます**。 更新したSPAをAEMにデプロイして、変更を確認します。

1. プロジェクトのルート(`aem-guides-wknd-spa`)に移動し、Mavenを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 更新されたロゴとスタイルが適用さ `Header` れていることが確認できます。

   ![AEMの更新されたヘッダー](./assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## ノード・サス・エラーのトラブルシューティング

開発の過程で、次のエラーが発生する場合があります。

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

これは、 **Node.jsのローカルバージョンと** npm **が、フロントエンドMaven-Pluginで使用されているバージョンと異なる場合に発生する可能性があり**[](https://github.com/eirslett/frontend-maven-plugin)ます。 このコマンドを実行すると、問題を一時的に修正したり、フォルダーを削除して `npm rebuild node-sass``ui.frontend/node_modules` 再インストールしたりできます。

これに対して、より恒久的に対処する方法もいくつかあります。

* npmとNode.jsのローカルバージョンが、 [Mavenビルドで使用されるバージョンと一致していることを確認します](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* 次の追加実行手順は、手順の前 `ui.frontend/pom.xml` に行い `npm run build` ます。

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## バリデーターが{#congratulations}

SPAを更新し、AEMとの統合を確認しました。 現在は、AEM JSONモデルAPIに対して **webpack-dev-serverを使用してSPAを開発する2つの異なる方法を知っています**。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/integrate-spa-solution`ます。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md) -AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。
