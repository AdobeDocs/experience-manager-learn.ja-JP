---
title: SPAの統合 | AEM SPA Editor と React の使用の手引き
description: React で記述されたシングルページアプリケーション (SPA) のソースコードをAdobe Experience Manager(AEM) プロジェクトと統合する方法を説明します。 webpack 開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSON モデル API に対するSPAを迅速に開発する方法を説明します。
sub-product: sites
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '1840'
ht-degree: 4%

---

# SPAの統合 {#developer-workflow}

React で記述されたシングルページアプリケーション (SPA) のソースコードをAdobe Experience Manager(AEM) プロジェクトと統合する方法を説明します。 webpack 開発サーバーなどの最新のフロントエンドツールを使用して、AEM JSON モデル API に対するSPAを迅速に開発する方法を説明します。

## 目的

1. SPAプロジェクトがAEMとクライアント側ライブラリを統合する方法を説明します。
2. 専用のフロントエンド開発に Webpack 開発サーバーを使用する方法を説明します。
3. の使用に関する詳細 **プロキシ** および静的 **mock** AEM JSON モデル API に対する開発用のファイル。

## 作成する内容

この章では、SPAとの統合方法を理解するために、AEMにいくつかの小さな変更を加えます。
この章では、 `Header` コンポーネントをSPAに追加します。 これを構築中 **静的** `Header` コンポーネントAEM SPA開発に対するいくつかの方法が使用されます。

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAを拡張して静的を追加します。 `Header` コンポーネント*

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment). この章は、 [プロジェクトを作成](create-project.md) ただし、必要な操作は、SPA対応AEMプロジェクトのみです。

## 統合アプローチ {#integration-approach}

AEMプロジェクトの一部として、次の 2 つのモジュールが作成されました。 `ui.apps` および `ui.frontend`.

この `ui.frontend` モジュールは [webpack](https://webpack.js.org/) すべてのSPAソースコードを含むプロジェクト。 SPAの開発およびテストの大部分は、webpack プロジェクトでおこなわれます。 実稼動ビルドがトリガーされると、SPAは webpack を使用して構築およびコンパイルされます。 コンパイル済みのアーティファクト（CSS および JavaScript）が `ui.apps` モジュールをAEMランタイムにデプロイします。

![ui.frontend の高レベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の概要です。*

フロントエンドビルドに関する追加情報は、次のとおりです。 [ここにある](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA統合 {#inspect-spa-integration}

次に、 `ui.frontend` モジュールを使用して、 [AEMプロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. 任意の IDE で、AEMプロジェクトを開きます。 このチュートリアルでは、 [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=ja#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

1. を展開して検査します。 `ui.frontend` フォルダー。 ファイルを開きます。 `ui.frontend/package.json`

1. 以下 `dependencies` 関連する `react` 含む `react-scripts`

   この `ui.frontend` は、 [React アプリを作成](https://create-react-app.dev/) または CRA（短く）。 この `react-scripts` version は、使用される CRA のバージョンを示します。

1. また、次のプレフィックスが付いた複数の依存関係もあります。 `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   上記のモジュールは、 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) とは、SPAコンポーネントをAEMコンポーネントにマッピングできる機能を提供します。

   また、次のものも含まれます。 [AEM WCM コンポーネント — React Core 実装](https://github.com/adobe/aem-react-core-wcm-components-base) および [AEM WCM コンポーネント — Spa エディター — React Core 実装](https://github.com/adobe/aem-react-core-wcm-components-spa). これらは、すぐに使用できるAEMコンポーネントのセットにマッピングされる、再利用可能な UI コンポーネントです。 これらは、そのまま使用するように設計され、プロジェクトのニーズに合わせてスタイルが設定されます。

1. 内 `package.json` ファイルには複数の `scripts` 定義済み：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   これらは、作成される標準のビルドスクリプトです [利用可能](https://create-react-app.dev/docs/available-scripts) React アプリの作成を参照してください。

   唯一の違いは `&& clientlib` から `build` スクリプト この追加の命令は、コンパイル済みのSPAを `ui.apps` ビルド時にクライアントサイドライブラリとしてモジュールを作成する。

   npm モジュール [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) を使用すると、この処理が容易になります。

1. `ui.frontend/clientlib.config.js` ファイルを検査します。この設定ファイルは、 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) を参照して、クライアントライブラリの生成方法を確認してください。

1. `ui.frontend/pom.xml` ファイルを検査します。このファイルは、 `ui.frontend` フォルダーを [Maven モジュール](https://maven.apache.org/guides/mini/guide-multiple-modules.html). この `pom.xml` ファイルを更新して [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) から **テスト** および **ビルド** Maven のビルド時にSPAが生成されます。

1. Inspectファイル `index.js` 時刻 `ui.frontend/src/index.js`:

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

   `index.js` は、SPAのエントリポイントです。 `ModelManager` は、AEM SPA Editor JS SDK で提供されます。 これは、 `pageModel` （JSON コンテンツ）をアプリケーションにコピーします。

1. Inspectファイル `import-component.js` 時刻 `ui.frontend/src/import-components.js`. このファイルは、標準のをインポートします **React コアコンポーネント** を使用して、プロジェクトで使用できるようにします。 次の章では、AEMコンテンツとSPAコンポーネントのマッピングを調べます。

## 静的なSPAコンポーネントの追加 {#static-spa-component}

次に、SPAに新しいコンポーネントを追加し、変更をローカルのAEMインスタンスにデプロイします。 これは、SPAの更新方法を示すための簡単な変更です。

1. 内 `ui.frontend` モジュール、下 `ui.frontend/src/components` 次の名前の新しいフォルダーを作成します。 `Header`.
1. という名前のファイルを作成します。 `Header.js` の下 `Header` フォルダー。

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

   上記は、静的テキスト文字列を出力する標準の React コンポーネントです。

1. `ui.frontend/src/App.js` ファイルを開きます。これは、アプリケーションのエントリポイントです。
1. 次の更新を行う： `App.js` 静的を含めるには `Header`:

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

1. 新しいターミナルを開き、 `ui.frontend` フォルダーを作成し、 `npm run build` コマンド：

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

1. `ui.apps` フォルダーに移動し、の下 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` コンパイル済みのSPAファイルが`ui.frontend/build` フォルダー。

   ![ui.apps で生成されたクライアントライブラリ](./assets/integrate-spa/compiled-spa-uiapps.png)

1. ターミナルに戻り、 `ui.apps` フォルダー。 次の Maven コマンドを実行します。

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

   これにより、 `ui.apps` AEMのローカル実行インスタンスにパッケージ化します。

1. ブラウザータブを開き、に移動します。 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). これで、 `Header` コンポーネントがSPAに表示されている状態。

   ![初期ヘッダー実装](./assets/integrate-spa/initial-header-implementation.png)

   上記の手順は、プロジェクトのルートから Maven ビルドをトリガーするとき ( つまり、 `mvn clean install -PautoInstallSinglePackage`) をクリックします。 これで、SPAとAEMのクライアント側ライブラリ間の統合の基本を理解する必要があります。 編集や追加は可能です。 `Text` 静的の下のAEM内のコンポーネント `Header` コンポーネント。

## Webpack Dev Server - JSON API のプロキシ {#proxy-json}

前の演習で見たように、クライアントライブラリをビルドして、AEMのローカルインスタンスに同期するには、数分かかります。 これは最終テストで使用できますが、SPAの開発の大部分には理想的ではありません。

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) を使用して、SPAを迅速に開発できます。 SPAは、AEMで生成された JSON モデルによって駆動されます。 この演習では、AEMの実行中のインスタンスからの JSON コンテンツを次のようにします。 **プロキシ化** を開発サーバーに送信します。

1. IDE に戻り、ファイルを開きます。 `ui.frontend/package.json`.

   次のような行を探します。

   ```json
   "proxy": "http://localhost:4502",
   ```

   この [React アプリを作成](https://create-react-app.dev/docs/proxying-api-requests-in-development) は、API リクエストをプロキシするための簡単なメカニズムを提供します。 すべての不明なリクエストは、を通じてプロキシされます `localhost:4502`ローカルのAEM Quickstart

1. ターミナルウィンドウを開き、 `ui.frontend` フォルダー。 `npm start` コマンドを実行します。

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

1. 新しいブラウザータブを開き（まだ開いていない場合）、に移動します。 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack 開発サーバー — プロキシ json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、オーサリング機能は有効になっていません。

   >[!NOTE]
   >
   > AEMのセキュリティ要件により、同じブラウザーで、別のタブでローカルのAEMインスタンス (http://localhost:4502) にログインする必要があります。

1. IDE に戻り、という名前のファイルを作成します。 `Header.css` 内 `src/components/Header` フォルダー。
1. 次の項目に `Header.css` を次のように設定します。

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

1. 再度開く `Header.js` 次の行を参照に追加します。 `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   変更内容を保存します。

1. に移動します。 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) スタイルの変更が自動的に反映されていることを確認するには、次の手順を実行します。

1. `Page.css`（`ui.frontend/src/components/Page`）ファイルを開きます。パディングを修正するには、次の変更を行います。

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. ブラウザーに戻る ( ) [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). アプリケーションの変更が直ちに反映されていることを確認します。

   ![ヘッダーにスタイルが追加されました](assets/integrate-spa/added-logo-localhost.png)

   AEMで引き続きコンテンツを更新し、それらを **webpack-dev-server**、コンテンツをプロキシしているので。

1. 次を使用して webpack 開発サーバーを停止します。 `ctrl+c` を設定します。

## AEMへのSPAアップデートのデプロイ

に加えられた変更 `Header` は現在、 **webpack-dev-server**. 更新したSPAをAEMにデプロイして、変更を確認します。

1. プロジェクトのルート (`aem-guides-wknd-spa`) をクリックし、Maven を使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. に移動します。 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 更新された `Header` スタイルを適用しました。

   ![AEMの更新済みヘッダー](assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## おめでとうございます。 {#congratulations}

これで、SPAを更新し、AEMとの統合を確認しました。 を使用してAEM JSON モデル API に対してSPAを開発する方法を知っています。 **webpack-dev-server**.

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md) - AEM SPA Editor JS SDK を使用して、React コンポーネントをAdobe Experience Manager(AEM) コンポーネントにマッピングする方法を説明します。 コンポーネントマッピングを使用すると、AEM SPAエディター内で、従来のAEMオーサリングと同様に、SPAコンポーネントを動的に更新できます。

## （ボーナス）Webpack Dev Server — モック JSON API {#mock-json}

迅速な開発のもう 1 つの方法は、静的 JSON ファイルを使用して JSON モデルとして機能させることです。 JSON を「モック」することで、ローカルのAEMインスタンスへの依存を削除します。 また、フロントエンド開発者は、機能をテストし、JSON API の変更を促すために JSON モデルを更新できます。この変更は、後でバックエンド開発者によって実装されます。

モック JSON の初期設定は次のようにおこないます。 **ローカルのAEMインスタンスが必要**.

1. IDE に戻り、に移動します。 `ui.frontend/public` をクリックし、 `mock-content`.
1. という名前の新しいファイルを作成します。 `mock.model.json` 下 `ui.frontend/public/mock-content`.
1. ブラウザーで、に移動します。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

1. 前の手順の JSON 出力をファイルに貼り付けます。 `mock.model.json`.

   ![モックモデル Json ファイル](./assets/integrate-spa/mock-model-json-created.png)

1. `index.html`（`ui.frontend/public/index.html`）ファイルを開きます。変数を指すようにAEMページモデルのメタデータプロパティを更新します。 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   変数を `cq:pagemodel_root_url` を使用すると、プロキシとモック json モデルを簡単に切り替えることができます。

1. ファイルを開きます。 `ui.frontend/.env.development` 次の更新を行い、次の値をコメントアウトします。 `REACT_APP_PAGE_MODEL_PATH` および `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 現在実行中の場合は、 **webpack-dev-server**. を開始します。 **webpack-dev-server** ターミナルから：

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   に移動します。 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) SPAと同じコンテンツが **プロキシ** json.

1. を少し変更します。 `mock.model.json` ファイルが作成されました。 更新されたコンテンツがすぐに **webpack-dev-server**.

   ![モモデル json の更新をモックする](./assets/integrate-spa/webpack-mock-model.gif)

JSON モデルを操作し、実稼働中のSPAに対する影響を確認できることは、開発者が JSON モデル API を理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行しておこなうことができます。

JSON コンテンツを使用する場所を切り替えることができるようになりました。 `env.development` ファイル：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
