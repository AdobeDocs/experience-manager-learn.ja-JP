---
title: SPAの統合 | AEM SPA EditorとReactの使い始めに
description: Reactで記述された単一ページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 AEM JSONモデルAPIに対してSPAを迅速に開発するために、webpack開発サーバーなどの最新のフロントエンドツールを使用する方法を説明します。
sub-product: サイト
feature: SPAエディタ
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2109'
ht-degree: 5%

---


# SPAを統合する{#integrate-spa}

Reactで記述された単一ページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 AEM JSONモデルAPIに対してSPAを迅速に開発するために、webpack開発サーバーなどの最新のフロントエンドツールを使用する方法を説明します。

## 目的

1. SPAプロジェクトがAEMとクライアント側ライブラリを統合する方法を理解します。
2. WebPack開発サーバーを使用して、専用のフロントエンド開発を行う方法を説明します。
3. AEM JSONモデルAPIに対する開発に&#x200B;**プロキシ**&#x200B;と静的&#x200B;**モック**&#x200B;ファイルの使用を調べます。

## 作成する内容

この章では、SPAに単純な`Header`コンポーネントを追加します。 この静的な`Header`コンポーネントを構築する過程で、AEM SPA開発に対するいくつかのアプローチが使用されます。

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的 `Header` コンポーネントが追加されます*

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

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

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)に常に表示できます。また、ブランチ`React/integrate-spa-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 統合アプローチ{#integration-approach}

2つのモジュールがAEMプロジェクトの一部として作成されました。`ui.apps`と`ui.frontend`。

`ui.frontend`モジュールは、SPAソースコードのすべてを含む[webpack](https://webpack.js.org/)プロジェクトです。 SPAの開発およびテストの大部分は、WebPackプロジェクトで行われます。 実稼働版ビルドがトリガーされると、SPAはWebPackを使用して構築およびコンパイルされます。 コンパイルされたアーティファクト（CSSとJavaScript）が`ui.apps`モジュールにコピーされ、AEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の高レベルな描写。*

フロントエンドビルドに関する追加情報は、[](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)を参照してください。

## InspectとSPAの統合{#inspect-spa-integration}

次に、`ui.frontend`モジュールを調べて、[AEMプロジェクトのアーキタイプ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)によって自動生成されたSPAを確認します。

1. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 このチュートリアルでは、[Visual StudioコードIDE](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)を使用します。

   ![VSCode - AEM WKND SPAプロジェクト](./assets/integrate-spa/vscode-ide-openproject.png)

2. `ui.frontend`フォルダーを展開し、調査します。 ファイル`ui.frontend/package.json`を開きます

3. `dependencies`の下には、`react-scripts`を含む`react`に関連するいくつかが見えるはずです

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend`は、[Create React App](https://create-react-app.dev/)またはCRA（短くはCRA）に基づくReactアプリケーションです。 `react-scripts`バージョンは、使用されるCRAのバージョンを示します。

4. また、`@adobe`というプリフィックスが付いた3つの依存関係もあります。

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上記のモジュールは、[AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)を構成し、SPAコンポーネントをAEMコンポーネントにマッピングする機能を提供します。

5. `package.json`ファイルには、次のように定義された`scripts`がいくつかあります。

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   これらは、Create React Appによって[利用可能な](https://create-react-app.dev/docs/available-scripts)にされた標準のビルドスクリプトです。

   唯一の違いは、`&& clientlib`を`build`スクリプトに追加することです。 この追加命令は、ビルド時にコンパイルされたSPAをクライアント側ライブラリとして`ui.apps`モジュールにコピーする役割を持ちます。

   npmモジュール[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)を使用して、この処理を容易にします。

6. `ui.frontend/clientlib.config.js` ファイルを検査します。この設定ファイルは、[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)でクライアントライブラリの生成方法を決定するために使用されます。

7. `ui.frontend/pom.xml` ファイルを検査します。このファイルは、`ui.frontend`フォルダーを[Mavenモジュール](http://maven.apache.org/guides/mini/guide-multiple-modules.html)に変換します。 `pom.xml`ファイルは、Mavenのビルド中に、[フロントエンド —maven-plugin](https://github.com/eirslett/frontend-maven-plugin)を&#x200B;**test**&#x200B;に、**build**&#x200B;に使用するように更新されました。

8. ファイル`index.js`を`ui.frontend/src/index.js`にInspect:

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

   `index.js` はSPAのエントリポイントです。`ModelManager` は、AEM SPAエディターJS SDKで提供されます。`pageModel`（JSONコンテンツ）を呼び出し、アプリケーションに挿入します。

## 追加ヘッダーコンポーネント{#header-component}

次に、SPAに新しいコンポーネントを追加し、変更をローカルのAEMインスタンスにデプロイします。

1. `ui.frontend`モジュールの`ui.frontend/src/components`の下に、`Header`という名前の新しいフォルダーを作成します。
2. `Header`フォルダーの下に`Header.js`という名前のファイルを作成します。

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

4. `ui.frontend/src/App.js` ファイルを開きます。これは、アプリケーションのエントリポイントです。
5. `App.js`を次のように更新し、静的`Header`を含めます。

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

6. 新しいターミナルを開き、`ui.frontend`フォルダーに移動して`npm run build`コマンドを実行します。

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

7. `ui.apps` フォルダーに移動し、`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`の下に、コンパイル済みのSPAファイルが`ui.frontend/build`フォルダーからコピーされているのが見えます。

   ![ui.appsで生成されたクライアントライブラリ](./assets/integrate-spa/compiled-spa-uiapps.png)

8. ターミナルに戻り、`ui.apps`フォルダーに移動します。 次のMavenコマンドを実行します。

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

   これにより、AEMのローカルで実行されているインスタンスに`ui.apps`パッケージが展開されます。

9. ブラウザーのタブを開き、[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 これで、SPAに`Header`コンポーネントの内容が表示されます。

   ![最初のヘッダー実装](./assets/integrate-spa/initial-header-implementation.png)

   手順6 ～ 8は、プロジェクトのルートからMavenビルド（`mvn clean install -PautoInstallSinglePackage`など）をトリガーするときに自動的に実行されます。 これで、SPAとAEMのクライアント側ライブラリの統合の基本を理解する必要があります。 AEMでは、静的な`Header`コンポーネントの下に`Text`コンポーネントを編集して追加することができます。

## Webpack Dev Server - JSON APIのプロキシ{#proxy-json}

前の練習で見たように、クライアントライブラリを構築してAEMのローカルインスタンスに同期する場合は、数分かかります。 これは、最終テストでは許容されますが、SPA開発の大部分には適していません。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)は、SPAを迅速に開発するのに使用できます。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、AEMの実行中のインスタンスからのJSONコンテンツを開発サーバーに&#x200B;**プロキシ**&#x200B;します。

1. IDEに戻り、ファイル`ui.frontend/package.json`を開きます。

   次のような行を探します。

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Create React App](https://create-react-app.dev/docs/proxying-api-requests-in-development)は、APIリクエストをプロキシする簡単なメカニズムです。 不明なリクエストはすべて、ローカルのAEM quickstartである`localhost:4502`を介してプロキシされます。

2. ターミナルウィンドウを開き、`ui.frontend`フォルダーに移動します。 `npm start` コマンドを実行します。

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

3. 新しいブラウザータブを開き（まだ開いていない場合）、[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動します。

   ![Webpack Devサーバー — プロキシjson](./assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、有効になっているオーサリング機能が1つもない場合は表示されます。

   >[!NOTE]
   >
   > AEMのセキュリティ要件により、同じブラウザーで、別のタブでローカルAEMインスタンス(http://localhost:4502)にログインする必要があります。

4. IDEに戻り、`media`という名前の新しいフォルダを`ui.frontend/src/media`に作成します。
5. 次のWKNDロゴをダウンロードして`media`フォルダーに追加します。

   ![WKNDロゴ](./assets/integrate-spa/wknd-logo-dk.png)

6. `Header.js`(`ui.frontend/src/components/Header/Header.js`)を開き、ロゴを読み込みます。

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. `Header.js`を次のように更新して、ロゴをヘッダーに含めます。

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

   変更を`Header.js`に保存します。

8. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)のブラウザに戻ります。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたロゴ](./assets/integrate-spa/added-logo-localhost.png)

   コンテンツをプロキシしているので、AEMでコンテンツを更新し続けて、**webpack-dev-server**&#x200B;に反映されているのを確認できます。

9. ターミナルで`ctrl+c`を使用してwebpack devサーバーを停止します。

## Webpack Dev Server - Mock JSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的なJSONファイルを使用してJSONモデルとして機能させることです。 JSONを「モッキング」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者がJSONモデルを更新して機能をテストし、変更をJSON APIに導き、後でバックエンド開発者が実装することになります。

モックJSONの最初の設定では、**ローカルのAEMインスタンス**&#x200B;が必要です。

1. ブラウザーで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)に移動します。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

2. IDEに戻って`ui.frontend/public`に移動し、`mock-content`という名前の新しいフォルダを追加します。
3. `ui.frontend/public/mock-content`の下に`mock.model.json`という名前の新しいファイルを作成します。 **手順1**&#x200B;のJSON出力をここに貼り付けます。

   ![モックモデルJSONファイル](./assets/integrate-spa/mock-model-json-created.png)

4. `index.html`（`ui.frontend/public/index.html`）ファイルを開きます。AEMページモデルのメタデータプロパティを更新して、変数`%REACT_APP_PAGE_MODEL_PATH%`を指すようにします。

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url`の値に変数を使用すると、プロキシとモックjsonモデルを簡単に切り替えることができます。

5. ファイル`ui.frontend/.env.development`を開き、次の更新を行って`REACT_APP_PAGE_MODEL_PATH`の以前の値をコメントアウトします。

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 現在実行中の場合は、**webpack-dev-server**&#x200B;を停止します。 ターミナルから&#x200B;**webpack-dev-server**&#x200B;を開始します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動すると、**proxy** jsonで使用されているのと同じ内容のSPAが表示されます。

7. 以前に作成した`mock.model.json`ファイルに少し変更を加えます。 更新されたコンテンツは、すぐに&#x200B;**webpack-dev-server**&#x200B;に反映されます。

   ![モデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

JSONモデルを操作して、本番用SPA上での影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行して行うこともできます。

`env.development`ファイル内のエントリを切り替えて、JSONコンテンツの使用先を切り替えることができるようになりました。

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Sassで追加のスタイル

React では、各コンポーネントを自己完結型のモジュールにすることが推奨されます。一般的には、コンポーネント間で同じCSSクラス名を再使用しないようにすることをお勧めします。これにより、プリプロセッサーはあまり強力ではありません。 このプロジェクトは、変数のような役に立ついくつかの機能に対して[Sass](https://sass-lang.com/)を使用します。 また、このプロジェクトは[SUIT CSSの命名規則](https://github.com/suitcss/suit/blob/master/doc/components.md)に従います。 SUIT は BEM（Block Element Modifier）記法の 1 つで、一貫性のある CSS ルールの作成に使用されます。

1. ターミナルウィンドウを開き、起動した場合は&#x200B;**webpack-dev-server**&#x200B;を停止します。 `ui.frontend`フォルダー内から、[Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet)をインストールする次のコマンドを入力します。

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   `sass`をピア依存関係としてインストールします。

   ```shell
   $ npm install sass --save
   ```

2. `normalize-scss`をインストールして、スタイルをブラウザー間で標準化します。

   ```shell
   $ npm install normalize-scss
   ```

3. **webpack-dev-server**&#x200B;を開始して、スタイルの更新をリアルタイムで確認できます。

   ```shell
   $ npm start
   ```

   JSONモデルAPIを処理するには、モックのプロキシ方法のいずれかを使用します。

4. IDEに戻り、`ui.frontend/src`の下に`styles`という名前の新しいフォルダを作成します。
5. `ui.frontend/src/styles`の下に`_variables.scss`という名前の新しいファイルを作成し、次の変数を設定します。

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

6. `ui.frontend/src/index.css`にあるファイル`index.css`の拡張子を&#x200B;**`index.scss`**&#x200B;に変更します。 内容を次の内容に置き換えます。

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

7. `ui.frontend/src/index.js`を更新して、再名前の`index.scss`を含めます。

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. `ui.frontend/src/components/Header`の下に`Header.scss`という名前の新しいファイルを作成します。 ファイルに以下のように入力します。

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

9. `Header.scss`を`Header.js`を更新して含める：

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. ブラウザに戻り、**webpack-dev-server**:[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![スタイル設定ヘッダー — webpack devサーバー](./assets/integrate-spa/styled-header.png)

   これで、`Header`コンポーネントに追加された更新済みのスタイルが表示されます。

## SPAアップデートをAEMに展開する

`Header`に加えられた変更は、現在&#x200B;**webpack-dev-server**&#x200B;を通してのみ表示されます。 更新したSPAをAEMにデプロイして、変更を確認します。

1. プロジェクトのルート(`aem-guides-wknd-spa`)に移動し、Mavenを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 更新された`Header`がロゴとスタイルが適用されていることがわかります。

   ![AEMの更新されたヘッダー](./assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## ノード・サス・エラーのトラブルシューティング

開発の過程で、次のエラーが発生する場合があります。

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

これは、**Node.js**&#x200B;と&#x200B;**npm**&#x200B;のローカルバージョンが、[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)で使用されているものと異なる場合に発生する可能性があります。 `npm rebuild node-sass`コマンドを実行すると、一時的に問題を修正したり、`ui.frontend/node_modules`フォルダーを削除して再インストールしたりできます。

これに対して、より恒久的に対処する方法もいくつかあります。

* npmとNode.jsのローカルバージョンが、[Mavenビルド](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)で使用されているバージョンと一致していることを確認します。
* 追加次の実行手順は、`npm run build`手順の前の`ui.frontend/pom.xml`に対して実行します。

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

## これで完了です! {#congratulations}

SPAを更新し、AEMとの統合を確認しました。 これで、**webpack-dev-server**&#x200B;を使用してAEM JSONモデルAPIに対してSPAを開発する2つの異なる方法がわかります。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)に常に表示できます。また、ブランチ`React/integrate-spa-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md) - AEM SPAエディターJS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。
