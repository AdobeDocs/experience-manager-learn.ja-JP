---
title: SPAの統合 | AEM SPA EditorとReactの概要
description: Reactで記述されたシングルページアプリケーション(SPA)のソースコードをAdobe Experience Manager(AEM)プロジェクトと統合する方法を説明します。 webpack開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAを迅速に開発する方法を説明します。
sub-product: サイト
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2107'
ht-degree: 5%

---


# SPA {#integrate-spa}の統合

Reactで記述されたシングルページアプリケーション(SPA)のソースコードをAdobe Experience Manager(AEM)プロジェクトと統合する方法を説明します。 webpack開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAを迅速に開発する方法を説明します。

## 目的

1. SPAプロジェクトとAEMをクライアント側ライブラリと統合する方法について説明します。
2. 専用のフロントエンド開発にWebpack開発サーバーを使用する方法を説明します。
3. AEM JSONモデルAPIに対する開発に、**proxy**&#x200B;と静的&#x200B;**mock**&#x200B;ファイルの使用を調べます

## 作成する内容

この章では、単純な`Header`コンポーネントをSPAに追加します。 この静的な`Header`コンポーネントを構築する過程で、AEM SPA開発に対していくつかのアプローチが使用されます。

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的コンポーネントが追加さ `Header` れます*

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

### コードの取得

1. このチュートリアルの開始点をGitからダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Mavenを使用して、コードベースをローカルのAEMインスタンスにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)を使用する場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)で完成したコードをいつでも表示したり、ブランチ`React/integrate-spa-solution`に切り替えてコードをローカルでチェックアウトしたりできます。

## 統合アプローチ{#integration-approach}

AEMプロジェクトの一部として、2つのモジュールが作成されました。`ui.apps`と`ui.frontend`が表示されます。

`ui.frontend`モジュールは、すべてのSPAソースコードを含む[webpack](https://webpack.js.org/)プロジェクトです。 SPAの開発およびテストの大部分は、webpackプロジェクトでおこなわれます。 実稼動ビルドがトリガーされると、SPAはwebpackを使用して構築およびコンパイルされます。 コンパイル済みのアーティファクト（CSSおよびJavaScript）が`ui.apps`モジュールにコピーされ、AEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の概要です。*

フロントエンドビルドに関する追加情報は、[こちら](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)を参照してください。

## Inspect SPA統合{#inspect-spa-integration}

次に、 `ui.frontend`モジュールを調べて、[AEMプロジェクトのアーキタイプ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)によって自動生成されたSPAを理解します。

1. 任意のIDEで、WKND SPA用のAEMプロジェクトを開きます。 このチュートリアルでは、[Visual Studio Code IDE](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)を使用します。

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. `ui.frontend`フォルダーを展開し、検査します。 ファイル`ui.frontend/package.json`を開きます。

3. `dependencies`の下に、`react-scripts`を含む`react`に関連する複数のが表示されます。

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend`は、[Reactアプリを作成](https://create-react-app.dev/)またはCRA（短くして）に基づくReactアプリケーションです。 `react-scripts`バージョンは、使用されるCRAのバージョンを示します。

4. また、`@adobe`というプレフィックスが付いた依存関係は3つあります。

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上記のモジュールは、[AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)を構成し、SPAコンポーネントをAEMコンポーネントにマッピングできるようにする機能を提供します。

5. `package.json`ファイルには、次のように複数の`scripts`が定義されています。

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   これらは、React Appを作成することで[利用可能な](https://create-react-app.dev/docs/available-scripts)の標準ビルドスクリプトです。

   唯一の違いは、`&& clientlib`を`build`スクリプトに追加することです。 この追加の命令は、ビルド中に、コンパイル済みのSPAをクライアント側ライブラリとして`ui.apps`モジュールにコピーする役割を持ちます。

   npmモジュール[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)を使用して、これを容易にします。

6. `ui.frontend/clientlib.config.js` ファイルを検査します。この設定ファイルは、[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)でクライアントライブラリの生成方法を決定するために使用されます。

7. `ui.frontend/pom.xml` ファイルを検査します。このファイルは、`ui.frontend`フォルダーを[Mavenモジュール](http://maven.apache.org/guides/mini/guide-multiple-modules.html)に変換します。 `pom.xml`ファイルが更新され、Mavenのビルド中に[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)を&#x200B;**test**&#x200B;および&#x200B;**build**&#x200B;にSPAを使用するようになりました。

8. `ui.frontend/src/index.js`にあるファイル`index.js`をInspectにします。

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

   `index.js` は、SPAのエントリポイントです。`ModelManager` は、AEM SPA Editor JS SDKで提供されます。これは、を呼び出し、アプリケーションに`pageModel` （JSONコンテンツ）を挿入する役割を果たします。

## ヘッダーコンポーネント{#header-component}を追加します。

次に、新しいコンポーネントをSPAに追加し、変更をローカルのAEMインスタンスにデプロイします。

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

   上記は、静的なテキスト文字列を出力する標準のReactコンポーネントです。

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

7. `ui.apps` フォルダーに移動し、`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`の下に、コンパイル済みのSPAファイルが`ui.frontend/build`フォルダーからコピーされているのがわかります。

   ![ui.appsで生成されたクライアントライブラリ](./assets/integrate-spa/compiled-spa-uiapps.png)

8. ターミナルに戻り、 `ui.apps`フォルダーに移動します。 次のMavenコマンドを実行します。

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

   これにより、AEMのローカル実行インスタンスに`ui.apps`パッケージがデプロイされます。

9. ブラウザータブを開き、[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 これで、`Header`コンポーネントの内容がSPAに表示されます。

   ![初期ヘッダー実装](./assets/integrate-spa/initial-header-implementation.png)

   手順6～8は、プロジェクトのルートからMavenビルドをトリガーすると（つまり`mvn clean install -PautoInstallSinglePackage`）自動的に実行されます。 これで、SPAとAEMのクライアント側ライブラリ間の統合の基本を理解する必要があります。 静的な`Header`コンポーネントの下のAEMでは、引き続き`Text`コンポーネントを編集および追加できます。

## Webpack Dev Server - JSON APIをプロキシする{#proxy-json}

前の演習で見たように、クライアントライブラリをビルドしてAEMのローカルインスタンスに同期するには、数分かかります。 これは最終テストでは許容されますが、SPA開発の大部分には理想的ではありません。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)を使用して、SPAを迅速に開発できます。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、AEMの実行中のインスタンスのJSONコンテンツが&#x200B;**プロキシ**&#x200B;されて開発サーバーに送られます。

1. IDEに戻り、ファイル`ui.frontend/package.json`を開きます。

   次のような行を探します。

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Reactアプリを作成](https://create-react-app.dev/docs/proxying-api-requests-in-development)は、APIリクエストをプロキシする簡単なメカニズムを提供します。 不明なリクエストはすべて、ローカルのAEMクイックスタートである`localhost:4502`を介してプロキシされます。

2. ターミナルウィンドウを開き、 `ui.frontend`フォルダーに移動します。 `npm start` コマンドを実行します。

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

   ![Webpack開発サーバー — プロキシjson](./assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、オーサリング機能が有効になっていないはずです。

   >[!NOTE]
   >
   > AEMのセキュリティ要件により、同じブラウザーで、別のタブでローカルのAEMインスタンス(http://localhost:4502)にログインする必要があります。

4. IDEに戻り、`media`という名前の新しいフォルダーを`ui.frontend/src/media`に作成します。
5. 次のWKNDロゴをダウンロードし、`media`フォルダーに追加します。

   ![WKNDロゴ](./assets/integrate-spa/wknd-logo-dk.png)

6. `ui.frontend/src/components/Header/Header.js`で`Header.js`を開き、ロゴを読み込みます。

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. `Header.js`を次のように更新し、ロゴをヘッダーの一部として含めます。

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

8. ブラウザー([http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html))に戻ります。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたロゴ](./assets/integrate-spa/added-logo-localhost.png)

   コンテンツをプロキシしているので、AEMで引き続きコンテンツを更新し、**webpack-dev-server**&#x200B;に反映されるのを確認できます。

9. ターミナルで`ctrl+c`を使用してwebpack開発サーバーを停止します。

## Webpack Dev Server — モックJSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的JSONファイルをJSONモデルとして機能させることです。 JSONを「モック」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者は、機能をテストし、JSON APIに対する変更を促進するためにJSONモデルを更新でき、後でバックエンド開発者によって実装されます。

モックJSONの初期設定には、ローカルのAEMインスタンス&#x200B;**が必要です。**

1. ブラウザーで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)に移動します。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

2. IDEに戻り、`ui.frontend/public`に移動し、`mock-content`という名前の新しいフォルダーを追加します。
3. `ui.frontend/public/mock-content`の下に`mock.model.json`という名前の新しいファイルを作成します。 **手順1**&#x200B;のJSON出力をここに貼り付けます。

   ![モックモデルJsonファイル](./assets/integrate-spa/mock-model-json-created.png)

4. `index.html`（`ui.frontend/public/index.html`）ファイルを開きます。変数`%REACT_APP_PAGE_MODEL_PATH%`を指すようにAEMページモデルのメタデータプロパティを更新します。

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url`の値に変数を使用すると、プロキシとモックjsonモデルの切り替えが容易になります。

5. ファイル`ui.frontend/.env.development`を開き、次の更新を行って`REACT_APP_PAGE_MODEL_PATH`の以前の値をコメントアウトします。

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 現在実行中の場合は、**webpack-dev-server**&#x200B;を停止します。 ターミナルから&#x200B;**webpack-dev-server**&#x200B;を起動します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動すると、**proxy** jsonで使用されているのと同じ内容のSPAが表示されます。

7. 前に作成した`mock.model.json`ファイルに小さな変更を加えます。 更新されたコンテンツが&#x200B;**webpack-dev-server**&#x200B;にすぐに反映されます。

   ![モックモデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

JSONモデルを操作し、実際のSPAへの影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行しておこなうこともできます。

`env.development`ファイル内のエントリを切り替えて、JSONコンテンツを使用する場所を切り替えることができるようになりました。

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Sassでスタイルを追加

React では、各コンポーネントを自己完結型のモジュールにすることが推奨されます。一般的に、コンポーネント間で同じCSSクラス名を再利用しないようにすることをお勧めします。これにより、プリプロセッサーの使用が強力ではありません。 このプロジェクトでは、変数などの便利な機能を使用するために[Sass](https://sass-lang.com/)を使用します。 また、このプロジェクトは[SUIT CSSの命名規則](https://github.com/suitcss/suit/blob/master/doc/components.md)に大体従います。 SUIT は BEM（Block Element Modifier）記法の 1 つで、一貫性のある CSS ルールの作成に使用されます。

1. ターミナルウィンドウを開き、起動した場合は&#x200B;**webpack-dev-server**&#x200B;を停止します。 `ui.frontend`フォルダー内から、[Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet)をインストールする次のコマンドを入力します。

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   `sass`をピア依存関係としてインストールします。

   ```shell
   $ npm install sass --save
   ```

2. ブラウザー間でスタイルを正規化するには、`normalize-scss`をインストールします。

   ```shell
   $ npm install normalize-scss
   ```

3. **webpack-dev-server**&#x200B;を起動して、スタイルの更新をリアルタイムで確認できるようにします。

   ```shell
   $ npm start
   ```

   JSONモデルAPIの処理には、モックのプロキシアプローチのいずれかを使用します。

4. IDEに戻り、`ui.frontend/src`の下に`styles`という名前の新しいフォルダーを作成します。
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

6. `ui.frontend/src/index.css`にあるファイル拡張子`index.css`の名前を&#x200B;**`index.scss`**&#x200B;に変更します。 内容を次のように置き換えます。

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

9. `Header.js`を更新して`Header.scss`を含めます。

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. ブラウザーに戻り、**webpack-dev-server**:[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![スタイル設定ヘッダー — webpack devサーバー](./assets/integrate-spa/styled-header.png)

   更新されたスタイルが`Header`コンポーネントに追加されているのがわかります。

## AEMへのSPAアップデートのデプロイ

`Header`に加えられた変更は、現在、**webpack-dev-server**&#x200B;でのみ表示されます。 更新したSPAをAEMにデプロイして、変更を確認します。

1. プロジェクトのルート(`aem-guides-wknd-spa`)に移動し、Mavenを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 ロゴとスタイルが適用された更新済みの`Header`が表示されます。

   ![AEMのヘッダーの更新](./assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## node-sassエラーのトラブルシューティング

開発の過程で、次のエラーが発生する可能性があります。

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

これは、**Node.js**&#x200B;と&#x200B;**npm**&#x200B;のローカルバージョンが、[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)で使用されるものと異なる場合に発生する可能性があります。 コマンド`npm rebuild node-sass`を実行すると、問題を一時的に修正したり、`ui.frontend/node_modules`フォルダーを削除して再インストールしたりできます。

これに対して、より恒久的に対処する方法もいくつかあります。

* npmとNode.jsのローカルバージョンが、[Mavenビルド](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)で使用されているバージョンと一致していることを確認します。
* `npm run build`ステップの前に、次の実行ステップを`ui.frontend/pom.xml`に追加します。

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

## バリデーターが {#congratulations}

これで、SPAを更新し、AEMとの統合を確認しました。 **webpack-dev-server**&#x200B;を使用してAEM JSONモデルAPIに対するSPAを開発する方法が2つあります。

[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)で完成したコードをいつでも表示したり、ブランチ`React/integrate-spa-solution`に切り替えてコードをローカルでチェックアウトしたりできます。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md)  - AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法を説明します。コンポーネントマッピングを使用すると、AEM SPA Editor内で、従来のAEMオーサリングと同様に、SPAコンポーネントを動的に更新できます。
