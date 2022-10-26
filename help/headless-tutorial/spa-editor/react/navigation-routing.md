---
title: ナビゲーションとルーティングを追加 | AEM SPA Editor と React の使用の手引き
description: SPA Editor SDK を使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法について説明します。 動的ナビゲーションは、React Router と React Core Components を使用して実装されています。
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 1%

---

# ナビゲーションとルーティングを追加 {#navigation-routing}

SPA Editor SDK を使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法について説明します。 動的ナビゲーションは、React Router と React Core Components を使用して実装されています。

## 目的

1. SPA Editor を使用する場合に使用できるSPAモデルのルーティングオプションについて説明します。
1. 使用方法を学ぶ [React Router](https://reacttraining.com/react-router/) をクリックして、SPAの様々なビュー間を移動します。
1. AEM React コアコンポーネントを使用して、AEMページ階層に基づく動的ナビゲーションを実装します。

## 作成する内容

この章では、AEMのSPAにナビゲーションを追加します。 ナビゲーションメニューはAEMのページ階層によって駆動され、 [Navigation コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![ナビゲーションが追加されました](assets/navigation-routing/navigation-added.png)

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment). この章は、 [コンポーネントをマッピング](map-components.md) ただし、必要な操作をすべて実行するには、SPA対応AEMプロジェクトをローカルAEMインスタンスにデプロイする必要があります。

## テンプレートにナビゲーションを追加する {#add-navigation-template}

1. ブラウザーを開き、AEMにログインします。 [http://localhost:4502/](http://localhost:4502/). 開始コードベースは、既にデプロイされている必要があります。
1. 次に移動： **SPA Page Template**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 最外側を選択 **ルートレイアウトコンテナ** をクリックします。 **ポリシー** アイコン 注意 **not** をクリックし、 **レイアウトコンテナ** オーサリング用にロック解除されました。

   ![ルートレイアウトコンテナポリシーアイコンを選択](assets/navigation-routing/root-layout-container-policy.png)

1. という名前の新しいポリシーを作成します。 **SPA構造**:

   ![SPA構造ポリシー](assets/navigation-routing/spa-policy-update.png)

   の下 **許可されたコンポーネント** > **一般** /を選択します。 **レイアウトコンテナ** コンポーネント。

   の下 **許可されたコンポーネント** > **WKND SPA REACT — 構造** /を選択します。 **ナビゲーション** コンポーネント：

   ![ナビゲーションコンポーネントを選択](assets/navigation-routing/select-navigation-component.png)

   の下 **許可されたコンポーネント** > **WKND SPA REACT — コンテンツ** /を選択します。 **画像** および **テキスト** コンポーネント。 合計 4 つのコンポーネントを選択する必要があります。

   「**完了**」をクリックして、変更を保存します。

1. ページを更新し、 **ナビゲーション** ロック解除されたコンポーネントの上の **レイアウトコンテナ**:

   ![ナビゲーションコンポーネントをテンプレートに追加](assets/navigation-routing/add-navigation-component.png)

1. を選択します。 **ナビゲーション** コンポーネントとその **ポリシー** アイコンをクリックして、ポリシーを編集します。
1. を使用して新しいポリシーを作成 **ポリシーのタイトル** / **SPA Navigation**.

   以下 **プロパティ**:

   * を **ナビゲーションルート** から `/content/wknd-spa-react/us/en`.
   * を **ルートレベルを除外** から **1**.
   * オフ **すべての子ページを収集**.
   * を **ナビゲーション構造の深さ** から **3**.

   ![ナビゲーションポリシーの構成](assets/navigation-routing/navigation-policy.png)

   これにより、の 2 レベル下のナビゲーションが収集されます。 `/content/wknd-spa-react/us/en`.

1. 変更を保存すると、 `Navigation` テンプレートの一部として：

   ![入力済みのナビゲーションコンポーネント](assets/navigation-routing/populated-navigation.png)

## 子ページを作成

次に、AEMで別のビューとして機能する追加のページを作成します。 また、AEMが提供する JSON モデルの階層構造も調べます。

1. 次に移動： **サイト** コンソール： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). を選択します。 **WKND SPA React ホームページ** をクリックし、 **作成** > **ページ**:

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

1. の下 **テンプレート** 選択 **SPA Page**. の下 **プロパティ** 入力 **1 ページ目** の **タイトル** および **page-1** 名前として。

   ![最初のページのプロパティを入力](assets/navigation-routing/initial-page-properties.png)

   クリック **作成** ダイアログのポップアップで、 **開く** をクリックして、AEM SPA Editor でページを開きます。

1. 新しい **テキスト** コンポーネントをメインに **レイアウトコンテナ**. コンポーネントを編集し、次のテキストを入力します。 **1 ページ目** RTE と **H2** 要素。

   ![サンプルコンテンツページ 1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

1. AEM Sitesコンソールに戻り、上記の手順を繰り返して、という名前の 2 番目のページを作成します。 **2 ページ目** 兄弟として **1 ページ目**.
1. 最後に、3 番目のページを作成します。 **3 ページ目** しかし **子** / **2 ページ目**. 完了すると、サイト階層は次のようになります。

   ![サイト階層のサンプル](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. ナビゲーションコンポーネントを使用して、SPAの様々な領域に移動できるようになりました。

   ![ナビゲーションとルーティング](assets/navigation-routing/navigation-working.gif)

1. AEM Editor の外部でページを開きます。 [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 以下を使用： **ナビゲーション** コンポーネントを使用して、アプリの様々なビューに移動できます。

1. ブラウザーの開発者ツールを使用して、移動中にネットワークリクエストを調べます。 以下のスクリーンショットは、Google Chrome ブラウザーからキャプチャしたものです。

   ![ネットワーク要求の監視](assets/navigation-routing/inspect-network-requests.png)

   最初のページ読み込みの後、後続のナビゲーションでは完全なページ更新がおこなわれず、以前に訪問したページに戻る際にネットワークトラフィックが最小化されることを確認します。

## 階層ページの JSON モデル {#hierarchy-page-json-model}

次に、SPAのマルチビューエクスペリエンスを推進する JSON モデルを調べます。

1. 新しいタブで、AEMが提供する JSON モデル API を開きます。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). ブラウザー拡張機能を使用すると、 [JSON の形式を設定する](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   この JSON コンテンツは、SPAが最初に読み込まれる際にリクエストされます。 外側の構造は次のようになります。

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   の下 `:children` 作成した各ページのエントリが表示されます。 すべてのページのコンテンツは、この最初の JSON リクエストに含まれます。 ナビゲーションルーティングを使用すると、コンテンツは既にクライアント側で使用可能なので、SPAの後続のビューが迅速に読み込まれます。

   読み込むのは賢明ではない **すべて** SPAのコンテンツを最初の JSON リクエストで取得すると、最初のページの読み込みが遅くなります。 次に、ページの階層の深さを収集する方法を見てみましょう。

1. 次に移動： **SPA Root** テンプレートの場所： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   次をクリック： **ページプロパティメニュー** > **ページポリシー**:

   ![SPA Root のページポリシーを開きます。](assets/navigation-routing/open-page-policy.png)

1. この **SPA Root** テンプレートに追加の **階層構造** タブをクリックして、収集される JSON コンテンツを制御します。 この **構造の深さ** は、サイト階層内での子ページの収集レベルを **root**. また、 **構造パターン** フィールドを使用して、正規表現に基づいて追加のページを除外します。

   を更新します。 **構造の深さ** から **2**:

   ![構造の深さを更新](assets/navigation-routing/update-structure-depth.png)

   クリック **完了** をクリックして、ポリシーに対する変更を保存します。

1. JSON モデルを再度開く [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   この **3 ページ目** パスが削除されました： `/content/wknd-spa-react/us/en/home/page-2/page-3` を最初の JSON モデルから取得します。 これは、 **3 ページ目** は階層のレベル 3 にあり、レベル 2 の最大深さのコンテンツのみを含めるようにポリシーを更新しました。

1. SPAホームページを再度開きます。 [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) ブラウザーの開発者ツールを開きます。

   ページを更新すると、次の XHR リクエストが表示されます。 `/content/wknd-spa-react/us/en.model.json`:SPAのルート。 このチュートリアルで前述したSPAルートテンプレートの階層の深さ設定に基づいて、3 つの子ページのみが含まれます。 これには、 **3 ページ目**.

   ![最初の JSON リクエスト — SPA Root](assets/navigation-routing/initial-json-request.png)

1. 開発者ツールを開いた状態で、 `Navigation` 直接移動先のコンポーネント **3 ページ目**:

   次に対する新しい XHR リクエストがおこなわれることを確認します。 `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![3 ページ目の XHR リクエスト](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager は、 **3 ページ目** JSON コンテンツは使用できず、追加の XHR リクエストを自動的にトリガー化します。

1. 次の場所に直接移動して、ディープリンクを試してみます。 [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). また、ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## Inspect React Routing  {#react-routing}

ナビゲーションとルーティングは、 [React Router](https://reactrouter.com/). React Router は、React アプリケーション用のナビゲーションコンポーネントの集まりです。 [AEM React コアコンポーネント](https://github.com/adobe/aem-react-core-wcm-components-base) は、React Router の機能を使用して **ナビゲーション** 前の手順で使用したコンポーネント。

次に、React Router がSPAとどのように統合されているかを調べ、React Router の [リンク](https://reactrouter.com/web/api/Link) コンポーネント。

1. IDE でファイルを開きます。 `index.js` 時刻 `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
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
   ```

   この `App` が `Router` コンポーネント [React Router](https://reacttraining.com/react-router/). この `ModelManager`はAEM SPA Editor JS SDK で提供され、JSON モデル API に基づいて動的ルートをAEMページに追加します。

1. ファイルを開きます。 `Page.js` 時刻 `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   この `Page` SPAコンポーネントは `MapTo` マップする関数 **ページ** をAEMから対応するSPAコンポーネントに追加します。 この `withRoute` ユーティリティは、 `cqPath` プロパティ。

1. を開きます。 `Header.js` コンポーネント `ui.frontend/src/components/Header/Header.js`.
1. を更新します。 `Header` 包む `<h1>` タグを [リンク](https://reactrouter.com/web/api/Link) ホームページに移動します。

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   デフォルトの `<a>` 使用するアンカータグ `<Link>` React Router が提供します。 ( `to=` が有効なルートを指す場合、SPAはそのルートに切り替わり、 **not** ページ全体の更新を実行します。 ここでは、単にホームページへのリンクをハードコードして、 `Link`.

1. 次の場所でテストを更新します。 `App.test.js` 時刻 `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   React Router の機能は、 `App.js` 単体テストを更新して、それを考慮する必要があります。

1. ターミナルを開き、プロジェクトのルートに移動し、Maven スキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEMのSPAのいずれかのページに移動します。 [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   を使用する代わりに、 `Navigation` 移動するコンポーネント、 `Header`.

   ![ヘッダーリンク](assets/navigation-routing/header-link.png)

   完全なページ更新が **not** トリガーされ、SPAルーティングが機能している。

1. 必要に応じて、 `Header.js` 標準を使用したファイル `<a>` アンカータグ：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   これは、SPAルーティングと通常の Web ページリンクの違いを説明するのに役立ちます。

## おめでとうございます。 {#congratulations}

これで、SPA Editor SDK でAEMページにマッピングすることで、SPAの複数のビューをサポートする方法を学びました。 React Router を使用して動的ナビゲーションが実装され、 `Header` コンポーネント。
