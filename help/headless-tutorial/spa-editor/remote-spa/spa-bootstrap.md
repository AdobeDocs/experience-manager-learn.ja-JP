---
title: SPA Editor用のリモートSPAのBootstrap
description: AEM SPA Editorの互換性を確保するためにリモートSPAをブートストラップする方法を説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 1%

---


# SPA Editor用のリモートSPAのBootstrap

編集可能な領域をリモートSPAに追加する前に、AEM SPA Editor JavaScript SDKおよびその他のいくつかの設定でブートストラップする必要があります。

## AEM SPA Editor JS SDK npm依存関係の追加

まず、AEM SPA npmの依存関係をReactプロジェクトに追加します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install --save \
    @adobe/aem-spa-page-model-manager \
    @adobe/aem-spa-component-mapping \
    @adobe/aem-react-editable-components \
    @adobe/aem-core-components-react-base \
    @adobe/aem-core-components-react-spa
```

+ `@adobe/aem-spa-page-model-manager` は、AEMからコンテンツを取得するためのAPIを提供します。
+ `@adobe/aem-spa-component-mapping` は、AEMコンテンツをSPAコンポーネントにマッピングするAPIを提供します。
+ ` @adobe/aem-react-editable-components` は、カスタムSPAコンポーネントを作成するためのAPIを提供し、Reactコンポーネントなどの一般的な実装を `AEMPage` 提供します。
+ `@adobe/aem-core-components-react-base` は、AEM WCMコアコンポーネントとシームレスに統合できる、使いやすいReactコンポーネントのスイートを提供し、SPA Editorに依存しません。主に、次のようなコンテンツコンポーネントが含まれます。
   + タイトル
   + テキスト
   + パンくずリスト
   + など。
+ `@adobe/aem-core-components-react-spa` は、AEM WCMコアコンポーネントとシームレスに統合できるが、SPA Editorが必要な、使いやすいReactコンポーネントのスイートを提供します。主に、次のような`@adobe/aem-core-components-react-base`のコンテンツコンポーネントを含むコンポーネントが含まれます。
   + コンテナ
   + カルーセル
   + など。

## SPA環境変数の確認

AEMとのやり取り方法を把握できるように、いくつかの環境変数をリモートSPAに公開する必要があります。

1. IDEの`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`にあるリモートSPAプロジェクトを開きます。
1. ファイル`.env.development`を開きます。
1. キーに特に注意して、ファイルを追加します。

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   ![リモートSPA環境変数](./assets/spa-bootstrap/env-variables.png)

   *Reactのカスタム環境変数の前にを付ける必要がありま `REACT_APP_`す。*

   + `REACT_APP_AEM_URI`:リモートSPAが接続するAEMサービスのスキームとホスト。
      + この値は、AEM環境（ローカル、開発、ステージ、実稼動）とAEMサービスのタイプ（オーサーとパブリッシュ）に応じて変わります
   + `REACT_APP_AEM_AUTH`:SPAが使用する資格情報は、AEMに対する認証とコンテンツの取得に使用されます。
      + AEMオーサーで使用する場合は必須
      + AEMパブリッシュでの使用に必要な場合があります（コンテンツが保護されている場合）。
      + AEM SDKに対する開発では、基本認証を介したローカルアカウントがサポートされます。 これは、このチュートリアルで使用する方法です。
      + AEM as aCloud Serviceと統合する場合は、[アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)を使用します。

## ModelManager APIの統合

アプリでAEM SPA npmの依存関係を使用できるように、`ReactDOM.render(...)`が呼び出される前に、プロジェクトの`index.js`でAEM `ModelManager`を初期化します。

[ModelManager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager)は、編集可能なコンテンツを取得するためにAEMに接続します。

1. IDEでリモートSPAプロジェクトを開きます。
1. ファイル`src/index.js`を開きます。
1. `ModelManager`を追加し、`ReactDOM.render(..)`呼び出しの前に初期化します。

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

`src/index.js`ファイルは次のようになります。

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 内部SPAプロキシの設定

SPAでAEMから編集可能なコンテンツを調達する場合は、SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)に[内部プロキシを設定することをお勧めします。このプロキシは、適切なリクエストをAEMにルーティングするように設定します。 これは、[http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npmモジュールを使用しておこないます。このモジュールは、ベースWKND GraphQL Appによって既にインストールされています。

1. IDEでリモートSPAプロジェクトを開きます。
1. `src/proxy/setupProxy.spa-editor.auth.basic.js` にファイルを作成します。
1. 次のコードをファイルに追加しました。

   ```
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_AUTHORIZATION } = process.env;
   
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
               path.startsWith('/graphq') ||
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
           } else if (path.startsWith('/adventure:') && path.endsWith('.model.json')) {
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
                   auth: REACT_APP_AUTHORIZATION,
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

   `setupProxy.spa-editor.auth.basic.js`ファイルは次のようになります。

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   このプロキシ設定は、主に次の2つの処理をおこないます。

   1. SPAに対しておこなわれたプロキシ固有のリクエスト`http://localhost:3000`をAEM `http://localhost:4502`に対して
      + `toAEM(path, req)`で定義されているように、AEMが提供する必要があることを示すパターンに一致するパスを持つリクエストのみをプロキシします。
      + 対応するAEMページにSPAパスを書き換えます（`pathRewriteToAEM(path, req)`で定義）。
   1. `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`で定義されているように、AEMコンテンツへのアクセスを許可するために、すべてのリクエストにCORSヘッダーを追加します。
      + これを追加しない場合、SPAでAEMコンテンツを読み込む際にCORSエラーが発生します。

1. ファイル`src/setupProxy.js`を開きます。
1. 行`const proxy = require('./proxy/setupProxy.auth.basic')`をコメントアウトします。
1. 新しいプロキシ設定ファイルを指す行を追加します。

   ```
   // Proxy configuration for SPA Editor (and GraphQL) using Basic Auth
   const proxy = require('./proxy/setupProxy.spa-editor.auth.basic')
   ```

   `setupProxy.js`ファイルは次のようになります。

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

`src/setupProxy.js`や参照ファイルに対する変更は、SPAを再起動する必要があります。

## 静的SPAリソース

WKNDロゴや読み込みグラフィックなどの静的なSPAリソースは、Remote SPAホストから強制的に読み込むために、src URLを更新する必要があります。 相対パスのままにすると、SPAがオーサリング用にSPAエディターに読み込まれる際に、これらのURLはデフォルトでSPAではなくAEMホストを使用するので、下の図に示すように、404個のリクエストがおこなわれます。

![静的リソースの破損](./assets/spa-bootstrap/broken-static-resource.png)

この問題を解決するには、Remote SPAがホストする静的リソースに、Remote SPA originを含む絶対パスを使用します。

1. IDEでSPAプロジェクトを開きます
1. SPA環境変数ファイル`src/.env.development`を開き、SPAパブリックURIの変数を追加します。

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _AEMをCloud Serviceとしてデプロイする場合は、対応するファイルに対して同じ操作をおこなう必要が `.env` あります。_

1. ファイル`src/App.js`を開きます。
1. SPA環境変数からのSPAパブリックURIの読み込み

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. SPAに対して解決を強制するには、WKNDのロゴ`<img src=.../>`の前に`REACT_APP_PUBLIC_URI`を付けます。

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. `src/components/Loading.js`での画像の読み込みも同様に行います。

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. .. と`src/components/AdventureDetails.js`の戻るボタンの&#x200B;__2つのインスタンス__

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

`App.js`、`Loading.js`、`AdventureDetails.js`ファイルは次のようになります。

![静的リソース](./assets/spa-bootstrap/static-resources.png)

## AEMレスポンシブグリッド

SPAの編集可能な領域でSPA Editorのレイアウトモードをサポートするには、AEMレスポンシブグリッドCSSをSPAに統合する必要があります。 心配しない — このグリッドシステムは、編集可能なコンテナにのみ使用され、選択したグリッドシステムを使用して、SPAの残りの部分のレイアウトを駆動できます。

AEMレスポンシブグリッドSCSSファイルをSPAに追加します。

1. IDEでSPAプロジェクトを開きます
1. 次の2つのファイルをダウンロードして`src/styles`にコピーします。
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEMレスポンシブグリッドSCSSジェネレーター
   + [_grid-init.scss](./assets/spa-bootstrap/_grid.scss)
      + SPA固有のブレークポイント（デスクトップおよびモバイル）と列(12)を使用して、`_grid.scss`を呼び出します。
1. `src/App.scss`を開き、`./styles/grid-init.scss`を読み込みます。

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

`_grid.scss`ファイルと`_grid-init.scss`ファイルは次のようになります。

![AEMレスポンシブグリッドSCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

SPAには、AEMコンテナに追加されたコンポーネントのAEMレイアウトモードをサポートするために必要なCSSが含まれるようになりました。

## SPAの起動

SPAがAEMとの統合のためにブートストラップ処理されたので、SPAを実行して、どのように表示されるかを確認します。

1. コマンドラインで、SPAプロジェクトのルートに移動します。
1. 通常のコマンドを使用してSPAを起動します（まだ起動していない場合は`npm install`を実行します）。

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. [http://localhost:3000](http://localhost:3000)でSPAを参照します。 全てが良く見えるはず！

![http://localhost:3000上で動作するSPA](./assets/spa-bootstrap/localhost-3000.png)

## AEM SPA EditorでSPAを開く

[http://localhost:3000](http://localhost:3000)でSPAを実行した状態で、AEM SPA Editorを使用して開きます。 SPAではまだ編集できません。AEMでのSPAのみ検証します。

1. AEMオーサーにログインします。
1. __サイト/WKND App > us > en__&#x200B;に移動します。
1. __WKND App Home Page__&#x200B;を選択して「__編集__」をタップすると、SPAが表示されます。

   ![WKNDアプリのホームページを編集](./assets/spa-bootstrap/edit-home.png)

1. 右上のモード切り替えボタンを使用して、__プレビュー__&#x200B;に切り替えます。
1. SPA

   ![http://localhost:3000上で動作するSPA](./assets/spa-bootstrap/spa-editor.png)

## バリデーターが

AEM SPA Editorと互換性を持たせるために、リモートSPAをブートストラップしました。 次の方法がわかりました。

+ AEM SPA Editor JS SDK npmの依存関係をSPAプロジェクトに追加します
+ SPA環境変数の設定
+ ModelManager APIとSPAの統合
+ SPAの内部プロキシを設定し、適切なコンテンツリクエストをAEMにルーティングする
+ SPA Editorのコンテキストで解決される静的SPAリソースの問題への対処
+ AEMレスポンシブグリッドCSSを追加して、AEM編集可能コンテナでのレイアウト処理をサポート

## 次の手順

これで、AEM SPA Editorとの互換性のベースラインを確立し、編集可能な領域の導入を開始できます。 まず、SPAに[固定編集可能なコンポーネント](./spa-fixed-component.md)を配置する方法を見てみます。
