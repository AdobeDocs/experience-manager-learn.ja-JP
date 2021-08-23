---
title: SPAの統合 | AEM SPA EditorとReactの概要
description: Reactで記述されたシングルページアプリケーション(SPA)のソースコードをAdobe Experience Manager(AEM)プロジェクトと統合する方法を説明します。 webpack開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAを迅速に開発する方法を説明します。
sub-product: サイト
feature: SPA Editor
version: cloud-service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1843'
ht-degree: 3%

---


# SPAの統合 {#developer-workflow}

Reactで記述されたシングルページアプリケーション(SPA)のソースコードをAdobe Experience Manager(AEM)プロジェクトと統合する方法を説明します。 webpack開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAを迅速に開発する方法を説明します。

## 目的

1. SPAプロジェクトとAEMをクライアント側ライブラリと統合する方法について説明します。
2. 専用のフロントエンド開発にWebpack開発サーバーを使用する方法を説明します。
3. AEM JSONモデルAPIに対する開発に、**proxy**&#x200B;と静的な&#x200B;**mock**&#x200B;ファイルの使用を調べます。

## 作成する内容

この章では、SPAとの統合方法を理解するために、AEMに対していくつかの小さな変更を加えます。
この章では、単純な`Header`コンポーネントをSPAに追加します。 この&#x200B;**静的** `Header`コンポーネントを構築する過程で、AEM SPA開発にいくつかのアプローチが使用されます。

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的コンポーネントが追加さ `Header` れます*

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 この章は、「[プロジェクトを作成](create-project.md)」の章の続きですが、必要な操作は、SPA対応のAEMプロジェクトだけです。

## 統合アプローチ {#integration-approach}

AEMプロジェクトの一部として、2つのモジュールが作成されました。`ui.apps`と`ui.frontend`が表示されます。

`ui.frontend`モジュールは、すべてのSPAソースコードを含む[webpack](https://webpack.js.org/)プロジェクトです。 SPAの開発およびテストの大部分は、webpackプロジェクトでおこなわれます。 実稼動ビルドがトリガーされると、SPAはwebpackを使用して構築およびコンパイルされます。 コンパイル済みのアーティファクト（CSSおよびJavaScript）が`ui.apps`モジュールにコピーされ、AEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の概要です。*

フロントエンドビルドに関する追加情報は、[こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)を参照してください。

## InspectとSPAの統合 {#inspect-spa-integration}

次に、 `ui.frontend`モジュールを調べて、[AEMプロジェクトのアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)によって自動生成されたSPAを理解します。

1. 任意のIDEで、AEMプロジェクトを開きます。 このチュートリアルでは、[Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)を使用します。

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

1. `ui.frontend`フォルダーを展開し、検査します。 ファイル`ui.frontend/package.json`を開きます。

1. `dependencies`の下に、`react-scripts`を含む`react`に関連する複数のが表示されます。

   `ui.frontend`は、[Reactアプリを作成](https://create-react-app.dev/)またはCRA（短くして）に基づくReactアプリケーションです。 `react-scripts`バージョンは、使用されるCRAのバージョンを示します。

1. `@adobe`というプレフィックスが付いた依存関係もいくつかあります。

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   上記のモジュールは、[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)を構成し、SPAコンポーネントをAEMコンポーネントにマッピングできるようにする機能を提供します。

   また、[AEM WCM Components - React Core実装](https://github.com/adobe/aem-react-core-wcm-components-base)および[AEM WCM Components - Spa Editor - React Core実装](https://github.com/adobe/aem-react-core-wcm-components-spa)も含まれます。 これらは、すぐに使用できるAEMコンポーネントにマッピングされる、再利用可能なUIコンポーネントのセットです。 これらは、そのまま使用するように設計され、プロジェクトのニーズに合わせてスタイル設定されます。

1. `package.json`ファイルには、次のように複数の`scripts`が定義されています。

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

1. `ui.frontend/clientlib.config.js` ファイルを検査します。この設定ファイルは、[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)でクライアントライブラリの生成方法を決定するために使用されます。

1. `ui.frontend/pom.xml` ファイルを検査します。このファイルは、`ui.frontend`フォルダーを[Mavenモジュール](http://maven.apache.org/guides/mini/guide-multiple-modules.html)に変換します。 `pom.xml`ファイルが更新され、Mavenのビルド中に[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)を&#x200B;**test**&#x200B;および&#x200B;**build**&#x200B;にSPAを使用するようになりました。

1. `ui.frontend/src/index.js`にあるファイル`index.js`をInspectにします。

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

1. `ui.frontend/src/import-components.js`にある`import-component.js`ファイルをInspectします。 このファイルは、初期設定の&#x200B;**Reactコアコンポーネント**&#x200B;を読み込み、プロジェクトで使用できるようにします。 次の章では、AEMコンテンツとSPAコンポーネントのマッピングを調べます。

## 静的なSPAコンポーネントの追加 {#static-spa-component}

次に、新しいコンポーネントをSPAに追加し、変更をローカルのAEMインスタンスにデプロイします。 これは、SPAの更新方法を示すために、簡単な変更です。

1. `ui.frontend`モジュールの`ui.frontend/src/components`の下に、`Header`という名前の新しいフォルダーを作成します。
1. `Header`フォルダーの下に`Header.js`という名前のファイルを作成します。

   ![ヘッダーフォルダーとファイル](assets/create-project/header-folder-js.png)

1. `Header.js` に以下を入力します。

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

1. `ui.frontend/src/App.js` ファイルを開きます。これは、アプリケーションのエントリポイントです。
1. `App.js`を次のように更新し、静的`Header`を含めます。

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

1. 新しいターミナルを開き、`ui.frontend`フォルダーに移動して`npm run build`コマンドを実行します。

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

1. `ui.apps` フォルダーに移動し、`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`の下に、コンパイル済みのSPAファイルが`ui.frontend/build`フォルダーからコピーされているのがわかります。

   ![ui.appsで生成されたクライアントライブラリ](./assets/integrate-spa/compiled-spa-uiapps.png)

1. ターミナルに戻り、 `ui.apps`フォルダーに移動します。 次のMavenコマンドを実行します。

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

1. ブラウザータブを開き、[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 これで、`Header`コンポーネントの内容がSPAに表示されます。

   ![初期ヘッダー実装](./assets/integrate-spa/initial-header-implementation.png)

   上記の手順は、プロジェクトのルートからMavenビルドをトリガーすると（つまり`mvn clean install -PautoInstallSinglePackage`）、自動的に実行されます。 これで、SPAとAEMのクライアント側ライブラリ間の統合の基本を理解する必要があります。 静的な`Header`コンポーネントの下のAEMでは、引き続き`Text`コンポーネントを編集および追加できます。

## Webpack Dev Server - JSON APIのプロキシ {#proxy-json}

前の演習で見たように、クライアントライブラリをビルドしてAEMのローカルインスタンスに同期するには、数分かかります。 これは最終テストでは許容されますが、SPA開発の大部分には理想的ではありません。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)を使用して、SPAを迅速に開発できます。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、AEMの実行中のインスタンスのJSONコンテンツが&#x200B;**プロキシ**&#x200B;されて開発サーバーに送られます。

1. IDEに戻り、ファイル`ui.frontend/package.json`を開きます。

   次のような行を探します。

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Reactアプリを作成](https://create-react-app.dev/docs/proxying-api-requests-in-development)は、APIリクエストをプロキシする簡単なメカニズムを提供します。 不明なリクエストはすべて、ローカルのAEMクイックスタートである`localhost:4502`を介してプロキシされます。

1. ターミナルウィンドウを開き、 `ui.frontend`フォルダーに移動します。 `npm start` コマンドを実行します。

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

1. 新しいブラウザータブを開き（まだ開いていない場合）、[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動します。

   ![Webpack開発サーバー — プロキシjson](./assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、オーサリング機能が有効になっていないはずです。

   >[!NOTE]
   >
   > AEMのセキュリティ要件により、同じブラウザーで、別のタブでローカルのAEMインスタンス(http://localhost:4502)にログインする必要があります。

1. IDEに戻り、`src/components/Header`フォルダーに`Header.css`という名前のファイルを作成します。
1. `Header.css`に以下を入力します。

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. `Header.js`を再度開き、`Header.css`を参照する次の行を追加します。

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   変更内容を保存します。

1. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動して、スタイルの変更が自動的に反映されることを確認します。

1. `Page.css`（`ui.frontend/src/components/Page`）ファイルを開きます。パディングを修正するには、次の変更を行います。

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. ブラウザー([http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html))に戻ります。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたスタイル](assets/integrate-spa/added-logo-localhost.png)

   コンテンツをプロキシしているので、AEMで引き続きコンテンツを更新し、**webpack-dev-server**&#x200B;に反映されるのを確認できます。

1. ターミナルで`ctrl+c`を使用してwebpack開発サーバーを停止します。

## AEMへのSPAアップデートのデプロイ

`Header`に加えられた変更は、現在、**webpack-dev-server**&#x200B;でのみ表示されます。 更新したSPAをAEMにデプロイして、変更を確認します。

1. プロジェクトのルート(`aem-guides-wknd-spa`)に移動し、Mavenを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。 更新された`Header`とスタイルが適用されているのが確認できます。

   ![AEMのヘッダーの更新](assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## おめでとうございます。 {#congratulations}

これで、SPAを更新し、AEMとの統合を確認しました。 **webpack-dev-server**&#x200B;を使用してAEM JSONモデルAPIに対するSPAを開発する方法を知っています。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md)  - AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法を説明します。コンポーネントマッピングを使用すると、AEM SPA Editor内で、従来のAEMオーサリングと同様に、SPAコンポーネントを動的に更新できます。

## （ボーナス）Webpack Dev Server — モックJSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的JSONファイルをJSONモデルとして機能させることです。 JSONを「モック」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者は、機能をテストし、JSON APIに対する変更を促進するためにJSONモデルを更新でき、後でバックエンド開発者によって実装されます。

モックJSONの初期設定には、ローカルのAEMインスタンス&#x200B;**が必要です。**

1. IDEに戻り、`ui.frontend/public`に移動し、`mock-content`という名前の新しいフォルダーを追加します。
1. `ui.frontend/public/mock-content`の下に`mock.model.json`という名前の新しいファイルを作成します。
1. ブラウザーで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)に移動します。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

1. 前の手順のJSON出力をファイル`mock.model.json`に貼り付けます。

   ![モックモデルJsonファイル](./assets/integrate-spa/mock-model-json-created.png)

1. `index.html`（`ui.frontend/public/index.html`）ファイルを開きます。変数`%REACT_APP_PAGE_MODEL_PATH%`を指すようにAEMページモデルのメタデータプロパティを更新します。

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url`の値に変数を使用すると、プロキシとモックjsonモデルの切り替えが容易になります。

1. ファイル`ui.frontend/.env.development`を開き、次の更新を行って`REACT_APP_PAGE_MODEL_PATH`の以前の値をコメントアウトします。

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 現在実行中の場合は、**webpack-dev-server**&#x200B;を停止します。 ターミナルから&#x200B;**webpack-dev-server**&#x200B;を起動します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)に移動すると、**proxy** jsonで使用されているのと同じ内容のSPAが表示されます。

1. 前に作成した`mock.model.json`ファイルに小さな変更を加えます。 更新されたコンテンツが&#x200B;**webpack-dev-server**&#x200B;にすぐに反映されます。

   ![モックモデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

JSONモデルを操作し、実際のSPAへの影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行しておこなうこともできます。

`env.development`ファイル内のエントリを切り替えて、JSONコンテンツを使用する場所を切り替えることができるようになりました。

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
