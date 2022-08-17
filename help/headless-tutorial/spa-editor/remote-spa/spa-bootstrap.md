---
title: SPA Editor 用のリモートSPAのBootstrap
description: AEM SPA Editor の互換性を確保するためにリモートSPAをブートストラップする方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# SPA Editor 用のリモートSPAのBootstrap

編集可能な領域をリモートSPAに追加する前に、AEM SPA Editor JavaScript SDK およびその他のいくつかの設定でブートストラップ処理する必要があります。


## WKND アプリソースのダウンロード

まだおこなっていない場合は、WKND アプリのソースコードを Github.com からダウンロードし、このチュートリアルで実行するSPAに対する変更を含むブランチを切り替えます。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## AEM SPA Editor JS SDK npm の依存関係を確認する

まず、AEM SPA npm の React プロジェクトへの依存関係を確認します。

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) :は、AEMからコンテンツを取得するための API を提供します。
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) :は、AEMコンテンツをSPAコンポーネントにマッピングする API を提供します。
+ [`@adobe/aem-react-editable-components`](https://github.com/adobe/aem-react-editable-components) :は、カスタムSPAコンポーネントを作成するための API を提供し、 `AEMPage` React コンポーネント。
+ [`@adobe/aem-core-components-react-base`](https://github.com/adobe/aem-react-core-wcm-components-base) :は、AEM WCM コアコンポーネントとシームレスに統合できる、使いやすい React コンポーネントのスイートを提供し、SPA Editor に依存しません。 主に次のようなコンテンツコンポーネントが含まれます。
   + タイトル
   + テキスト
   + パンくずリスト
   + その他
+ [`@adobe/aem-core-components-react-spa`](https://github.com/adobe/aem-react-core-wcm-components-spa) :は、AEM WCM コアコンポーネントとシームレスに統合でき、SPAエディターが必要な、すぐに使用できる React コンポーネントのスイートを提供します。 主に、次のコンテンツコンポーネントを含むコンポーネントが含まれます。 `@adobe/aem-core-components-react-base`例：
   + コンテナ
   + カルーセル
   + その他

## SPA環境変数の確認

AEMとのやり取り方法を理解できるように、いくつかの環境変数をリモートSPAに公開する必要があります。

1. 次の場所にあるリモートSPAプロジェクトを開く `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` IDE 内
1. ファイルを開きます。 `.env.development`
1. ファイル内で、キーに特に注意します。

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![リモートSPA環境変数](./assets/spa-bootstrap/env-variables.png)

   *React のカスタム環境変数の前には、「 `REACT_APP_`.*

   + `REACT_APP_HOST_URI`:リモートSPAが接続するAEMサービスのスキームとホスト。
      + この値は、AEM環境（ローカル、開発、ステージング、実稼動）とAEMサービスのタイプ（オーサーとパブリッシュ）に応じて変わります
   + `REACT_APP_USE_PROXY`:これにより、react 開発サーバーに対して、AEMリクエスト ( `/content, /graphql, .model.json` using `http-proxy-middleware` モジュール。
   + `REACT_APP_AUTH_METHOD`:AEMが提供するリクエストの認証方法。オプションは「service-token」、「dev-token」、「basic」です。no-auth ユースケースの場合は空白のままにします。
      + AEM オーサーで使用する場合は必須
      + AEM パブリッシュで使用する場合に必要な場合があります（コンテンツが保護されている場合）
      + AEM SDK に対する開発では、Basic Auth を介したローカルアカウントをサポートします。 これは、このチュートリアルで使用する方法です。
      + AEM as a Cloud Serviceとの統合時に、 [アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`:AEM __ユーザー名__ AEMコンテンツの取得時にSPAが認証を受けます。
   + `REACT_APP_BASIC_AUTH_PASS`:AEM __パスワード__ AEMコンテンツの取得時にSPAが認証を受けます。

## ModelManager API の統合

アプリで使用可能なAEM SPA npm の依存関係を使用して、AEMを初期化します。 `ModelManager` プロジェクトの `index.js` 前 `ReactDOM.render(...)` が呼び出されます。

この [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) は、AEMに接続して編集可能なコンテンツを取得します。

1. IDE で Remote SPAプロジェクトを開きます。
1. ファイルを開きます。 `src/index.js`
1. インポートを追加 `ModelManager` そして、それを初期化してから `ReactDOM.render(..)` 呼び出し

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

この `src/index.js` ファイルは次のようになります。

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 内部SPAプロキシの設定

SPAでAEMから編集可能コンテンツをソーシングする場合は、 [SPAの内部プロキシ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)：適切なリクエストをAEMにルーティングするように設定されています。 これは、 [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm モジュール。ベース WKND GraphQL アプリによって既にインストールされています。

1. IDE で Remote SPAプロジェクトを開きます。
1. 次の場所にあるファイルを開きます。 `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. 次のコードを確認します。

   ```
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

   この `setupProxy.spa-editor.auth.basic.js` ファイルは次のようになります。

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   このプロキシ設定では、主に次の 2 つをおこないます。

   1. SPAに対しておこなわれたプロキシ固有のリクエスト `http://localhost:3000` AEM `http://localhost:4502`
      + AEMが提供する必要があることを示すパターンに一致するパスを持つリクエストのみをプロキシします。詳しくは、 `toAEM(path, req)`.
      + SPAのパスを対応するAEMページに書き換えます ( `pathRewriteToAEM(path, req)`
   1. AEMコンテンツへのアクセスを許可するために、で定義されているように、すべてのリクエストに CORS ヘッダーを追加します。 `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + これを追加しない場合、SPAでAEMコンテンツを読み込む際に CORS エラーが発生します。

1. ファイルを開きます。 `src/setupProxy.js`
1. を指す行を確認します。 `setupProxy.spa-editor.auth.basic` プロキシ設定ファイル：

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

   この `setupProxy.js` ファイルは次のようになります。

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

なお、 `src/setupProxy.js` または参照ファイルを使用する場合は、SPAを再起動する必要があります。

## 静的SPAリソース

WKND ロゴや読み込みグラフィックなどの静的SPAリソースは、Remote SPAホストから強制的に読み込むには、src URL を更新する必要があります。 相対 URL を残した場合、SPAがオーサリング用にSPAエディターに読み込まれる際に、これらの URL はデフォルトでSPAではなくAEMホストを使用し、次の図に示すように、404 要求がおこなわれます。

![壊れた静的リソース](./assets/spa-bootstrap/broken-static-resource.png)

この問題を解決するには、Remote SPAでホストされる静的リソースに、Remote SPA origin を含む絶対パスを使用するようにします。

1. IDE でSPAプロジェクトを開きます。
1. SPA環境変数ファイルを開く `src/.env.development` SPAパブリック URI 用の変数を追加します。

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _AEM as a Cloud Serviceにデプロイする場合は、対応する `.env` ファイル。_

1. ファイルを開きます。 `src/App.js`
1. SPA環境変数からSPAパブリック URI をインポートします

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. WKND ロゴのプレフィックスを設定 `<img src=.../>` と `REACT_APP_PUBLIC_URI` SPAに対する解決を強制します。

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. での画像の読み込みも同じ処理を実行します。 `src/components/Loading.js`

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

1. また、 __2 つのインスタンス__ の背面ボタン `src/components/AdventureDetails.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

この `App.js`, `Loading.js`、および `AdventureDetails.js` ファイルは次のようになります。

![静的リソース](./assets/spa-bootstrap/static-resources.png)

## AEM Responsive Grid

SPAの編集可能な領域でSPA Editor のレイアウトモードをサポートするには、AEMレスポンシブグリッド CSS をSPAに統合する必要があります。 心配しないでください。このグリッドシステムは、編集可能なコンテナにのみ適用でき、選択したグリッドシステムを使用して、残りのSPAのレイアウトを駆動できます。

AEMレスポンシブグリッドの SCSS ファイルをSPAに追加します。

1. IDE でSPAプロジェクトを開きます。
1. 次の 2 つのファイルをにダウンロードしてコピーします。 `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEMレスポンシブグリッド SCSS ジェネレーター
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + 呼び出し `_grid.scss` SPA固有のブレークポイント（デスクトップおよびモバイル）と列 (12) を使用する。
1. 開く `src/App.scss` およびインポート `./styles/grid-init.scss`

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

この `_grid.scss` および `_grid-init.scss` ファイルは次のようになります。

![AEM Responsive Grid SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

SPAコンテナに追加されたコンポーネントのAEMレイアウトモードをサポートするために必要な CSS がAEMに含まれるようになりました。

## SPAを起動

SPAとAEMの統合のためにブートストラップ処理が完了したので、SPAを実行して、どのように表示されるかを確認しましょう。

1. コマンドラインで、SPAプロジェクトのルートに移動します。
1. 通常のコマンドを使用してSPAを起動します（まだ実行していない場合）。

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. でSPAを参照 [http://localhost:3000](http://localhost:3000). すべてが良く見えるはずです。

![http://localhost:3000上で稼働するSPA](./assets/spa-bootstrap/localhost-3000.png)

## AEM SPA Editor でSPAを開く

SPAを [http://localhost:3000](http://localhost:3000)AEM SPA Editor を使用して開きます。 SPAではまだ編集できません。AEMでSPAを検証するだけです。

1. AEM オーサーにログインします。
1. に移動します。 __サイト/ WKND アプリ/米国/en__
1. を選択します。 __WKND アプリのホームページ__ とタップします。 __編集__&#x200B;と入力し、SPAが表示されます。

   ![WKND アプリホームページを編集](./assets/spa-bootstrap/edit-home.png)

1. 切り替え先 __プレビュー__ 右上のモード切り替えボタンの使用
1. SPA

   ![http://localhost:3000上で稼働するSPA](./assets/spa-bootstrap/spa-editor.png)

## おめでとうございます。

AEM SPA Editor と互換性を持たせるために、リモートSPAをブートストラップしました。 次の方法を理解できました。

+ AEM SPA Editor JS SDK npm の依存関係をSPAプロジェクトに追加します。
+ SPA環境変数の設定
+ ModelManager API とSPAの統合
+ SPAの内部プロキシを設定して、適切なコンテンツリクエストをAEMにルーティングします
+ SPA Editor のコンテキストで解決される静的SPAリソースの問題への対処
+ AEMレスポンシブグリッド CSS を追加して、AEMの編集可能なコンテナでのレイアウト処理をサポート

## 次の手順

これで、AEM SPA Editor との互換性のベースラインを達成したので、編集可能な領域の導入を開始できます。 まず、 [編集可能な固定コンポーネント](./spa-fixed-component.md) をSPAに追加します。
