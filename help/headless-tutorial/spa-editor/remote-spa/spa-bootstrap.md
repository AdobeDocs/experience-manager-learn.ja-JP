---
title: SPA エディター用のリモート SPA のBootstrap
description: AEM SPA エディターの互換性を確保するためにリモート SPA のブートストラップを行う方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 97%

---

# SPA エディター用のリモート SPA のBootstrap

{{spa-editor-deprecation}}

編集可能な領域をリモート SPA に追加する前に、AEM SPA エディター JavaScript SDK とその他のいくつかの設定でブートストラップ処理を行う必要があります。

## AEM SPA エディター JS SDK npm 依存関係のインストール

まず、React プロジェクトのAEM SPA npm の依存関係を確認してインストールします。

* [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager)：AEM からコンテンツを取得するための API を提供します。
* [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping)：AEM コンテンツを SPA コンポーネントにマッピングする API を提供します。
* [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components)：カスタム SPA コンポーネントを作成するための API と、`AEMPage` React コンポーネントなどの一般的な実装を提供します。

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## SPA 環境変数の確認

AEM とのやり取りの方法を理解できるように、いくつかの環境変数をリモート SPA に公開する必要があります。

* IDE の `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` でリモート SPA プロジェクトを開きます
* ファイル `.env.development` を開きます
* ファイルでキーに特に注意しながら、必要に応じて更新します。

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![リモート SPA 環境変数](./assets/spa-bootstrap/env-variables.png)

  *React のカスタム環境変数には、「`REACT_APP_`」の接頭辞が付いている必要があります。*

   * `REACT_APP_HOST_URI`：リモート SPA が接続する AEM サービスのスキームとホスト
      * この値は、AEM環境（ローカル、開発、ステージング、実稼動）と AEM サービスのタイプ（オーサーとパブリッシュ）に応じて変わります
   * `REACT_APP_USE_PROXY`：`http-proxy-middleware` モジュールを使用する `/content, /graphql, .model.json` などの AEM リクエストをプロキシするよう React 開発サーバーに指示することで、開発中の CORS の問題を回避できます
   * `REACT_APP_AUTH_METHOD`：AEM 経由のリクエストの認証方法。オプションは「service-token」、「dev-token」、「basic」です。認証なしのユースケースの場合は空白のままにします。
      * AEM 作成者で使用する場合は必須です
      * AEM パブリッシュでの使用で必要な場合があります（コンテンツが保護されている場合）
      * AEM SDK に対する開発では、Basic Auth を介してローカルアカウントをサポートします。これは、このチュートリアルで使用している方法です
      * AEM as a Cloud Serviceと統合する際は、[アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja)を使用します
   * `REACT_APP_BASIC_AUTH_USER`：AEM コンテンツの取得時に SPA が認証する AEM __ユーザー名__
   * `REACT_APP_BASIC_AUTH_PASS`：AEM コンテンツの取得時に SPA が認証する AEM __パスワード__

## ModelManager API の統合

アプリで利用できる AEM SPA npm の依存関係を使って、`ReactDOM.render(...)` が呼び出される前にプロジェクトの `index.js` でAEMの `ModelManager` を初期化します。

この [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) は、AEM に接続して編集可能なコンテンツを取得します。

1. IDE でリモート SPAプロジェクトを開きます。
1. ファイル `src/index.js` を開きます。
1. `ModelManager` の読み込みを追加し、`root.render(..)` 呼び出しの前に初期化します。

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

`src/index.js` ファイルは次のようになります。

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 内部 SPA プロキシの設定

編集可能な SPA を作成する際は、適切なリクエストを AEM にルーティングするように設定されている [SPA の内部プロキシ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)を設定することをお勧めします。これは、ベース WKND GraphQL アプリによって既にインストールされている [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm モジュールを使用して行います。

1. IDE でリモート SPAプロジェクトを開きます。
1. `src/proxy/setupProxy.spa-editor.auth.basic.js` でファイルを開きます。
1. ファイルを次のコードで更新します。

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') ||
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   `setupProxy.spa-editor.auth.basic.js` ファイルは次のようになります。

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   このプロキシ設定では、主に次の 2 つを行います。

   1. SPA（`http://localhost:3000`）から AEM `http://localhost:4502` に対して行われる特定のリクエストのプロキシ
      * `toAEM(path, req)` に定義されているとおり、プロキシの対象となるのは、AEM が提供する必要があることを示すパターンに一致するパスを持つリクエストのみです。
      * `pathRewriteToAEM(path, req)` に定義されているとおり、SPA のパスを同等の AEM ページに書き換えます。
   1. `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);` で定義されているとおり、すべてのリクエストに CORS ヘッダーを追加して、AEM コンテンツへのアクセスを許可します。
      * これを追加しない場合、SPA で AEM コンテンツを読み込む際に CORS エラーが発生します。

1. ファイル `src/setupProxy.js` を開きます。
1. `setupProxy.spa-editor.auth.basic` プロキシ設定ファイルを指している行を確認します。

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

なお、`src/setupProxy.js` またはその参照先ファイルに変更がある場合は、SPA を再起動する必要があります。

## 静的 SPA リソース

WKND ロゴなどの静的 SPA リソースやグラフィックの読み込みでは、src URL を更新させて、リモート SPA のホストから強制的に読み込む必要があります。SPA がオーサリング用に SPA エディターに読み込まれる際に相対指定のままにした場合、これらの URL はデフォルトで SPA ではなく AEM のホストを使用するようになります。その結果、次の画像に示すように 404 リクエストエラーが発生します。

![壊れた静的リソース](./assets/spa-bootstrap/broken-static-resource.png)

この問題を解決するには、リモート SPA でホストされる静的リソースで、リモート SPA のオリジンを含んだ絶対パスを使用するようにします。

1. IDE で SPA プロジェクトを開きます。
1. SPA の環境変数ファイル `src/.env.development` を開き、SPA のパブリック URI 用の変数を追加します。

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _AEM as a Cloud Service にデプロイする場合は、対応する `.env` ファイルに同じ操作を実行する必要があります。_

1. ファイル `src/App.js` を開きます。
1. SPA の環境変数から SPA のパブリック URI を読み込みます。

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. WKND ロゴ `<img src=.../>` の前に接頭辞として `REACT_APP_PUBLIC_URI` を付けて、SPA に対する解決を強制します。

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. `src/components/Loading.js` での画像の読み込みにも、同じ操作を行います。

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. また、`src/components/AdventureDetails.js` での「戻る」ボタンの __2 つのインスタンス__&#x200B;についても同様です。

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

`App.js`、`Loading.js` および `AdventureDetails.js` ファイルは次のようになります。

![静的リソース](./assets/spa-bootstrap/static-resources.png)

## AEM レスポンシブグリッド

SPA の編集可能な領域で SPA エディターのレイアウトモードをサポートするには、AEM のレスポンシブグリッド CSS を SPA に統合する必要があります。このグリッドシステムは編集可能なコンテナにのみに適用され、SPA のそれ以外の部分のレイアウトには任意のグリッドシステムを使用できるので、心配する必要はありません。

AEM レスポンシブグリッド SCSS ファイルを SPA に追加します。

1. IDE で SPA プロジェクトを開きます。
1. 次の 2 つのファイルをダウンロードして、`src/styles` にコピーします。
   * [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      * AEM レスポンシブグリッド SCSS ジェネレーター
   * [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * SPA 固有のブレークポイント（デスクトップおよびモバイル）と列（12）を使用して、`_grid.scss` を呼び出します。
1. `src/App.scss` を開いて、`./styles/grid-init.scss` を読み込みます。

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

`_grid.scss` および `_grid-init.scss` ファイルは次のようになります。

![AEM レスポンシブグリッド SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

AEM コンテナに追加されたコンポーネントの AEM レイアウトモードをサポートするために必要な CSS が SPA に含まれるようになりました。

## ユーティリティクラス

次のユーティリティクラスを React アプリプロジェクトにコピーします。

* [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) を `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js` に
* [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) を `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js` に
* [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) を `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js` に
* [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) を `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js` に

![リモート SPA ユーティリティクラス](./assets/spa-bootstrap/utility-classes.png)

## SPA の起動

これで、AEM との統合に向けた SPA のブートストラップが完了したので、SPA を実行して、どのように表示されるかを確認します。

1. コマンドラインで、SPA プロジェクトのルートに移動します。
1. 通常のコマンドを使用して SPA を起動します（まだ起動していない場合）。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. [http://localhost:3000](http://localhost:3000) で SPA を参照します。 すべてがうまく表示されるはずです。

![http://localhostで動作する SPA:3000](./assets/spa-bootstrap/localhost-3000.png)

## AEM SPA エディターで SPA を開く

SPA が [http://localhost:3000](http://localhost:3000) で動作している状態で、AEM SPA エディターを使用して SPA を開きます。 SPA ではまだ何も編集できません。AEM での SPA の動作を検証するだけです。

1. AEM オーサーにログインします。
1.  __Sites／WKND アプリ／米国／英語__&#x200B;に移動します。
1. __WKND アプリのホームページ__&#x200B;を選択して「__編集__」をタップすると、SPA が表示されます。

   ![WKND アプリホームページの編集](./assets/spa-bootstrap/edit-home.png)

1. 右上のモード切り替えボタンを使用して「__プレビュー__」に切り替えます
1. SPA の周りをクリックします

   ![http://localhostで動作する SPA:3000](./assets/spa-bootstrap/spa-editor.png)

## おめでとうございます。

AEM SPA エディターと互換性を持たせるために、リモート SPA でブートストラップを行います。次の方法を学習しました。

* AEM SPA エディター JS SDK npm の依存関係を SPA プロジェクトに追加します
* SPA の環境変数を設定します
* ModelManager API と SPA を統合します
* SPA の内部プロキシを設定して、適切なコンテンツリクエストを AEM にルーティングします
* SPA エディターのコンテキストで解決される静的 SPA リソースの問題に対処します
* AEM のレスポンシブグリッド CSS を追加して、AEM の編集可能なコンテナでのレイアウト処理をサポートします

## 次の手順

これで、AEM SPA エディターとの互換性のベースラインを達成したので、編集可能な領域の導入を開始できます。 まず、[編集可能な固定コンポーネント](./spa-fixed-component.md) を SPA に追加します。
