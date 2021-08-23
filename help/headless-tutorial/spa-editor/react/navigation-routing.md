---
title: ナビゲーションとルーティングの追加 | AEM SPA EditorとReactの概要
description: SPA Editor SDKを使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法について説明します。 動的なナビゲーションは、React RouterとReactコアコンポーネントを使用して実装されます。
sub-product: サイト
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---


# ナビゲーションとルーティングの追加 {#navigation-routing}

SPA Editor SDKを使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法について説明します。 動的なナビゲーションは、React RouterとReactコアコンポーネントを使用して実装されます。

## 目的

1. SPA Editorを使用する場合に使用できるSPAモデルのルーティングオプションを理解します。
1. [React Router](https://reacttraining.com/react-router/)を使用して、SPAの異なるビュー間を移動する方法を説明します。
1. AEM Reactコアコンポーネントを使用して、AEMページ階層に基づく動的なナビゲーションを実装します。

## 作成する内容

この章では、AEMのSPAにナビゲーションを追加します。 ナビゲーションメニューはAEMページ階層によって駆動され、[ナビゲーションコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)が提供するJSONモデルを利用します。

![ナビゲーションの追加](assets/navigation-routing/navigation-added.png)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 この章は、「[コンポーネントのマップ](map-components.md)」の章の続きですが、必要な操作に従うのは、ローカルのAEMインスタンスにデプロイされるSPA対応のAEMプロジェクトです。

## テンプレートへのナビゲーションの追加 {#add-navigation-template}

1. ブラウザーを開き、 AEM [http://localhost:4502/](http://localhost:4502/)にログインします。 開始コードベースは、既にデプロイされている必要があります。
1. **SPA Page Template**&#x200B;に移動します。[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 最も外側にある&#x200B;**ルートレイアウトコンテナ**&#x200B;を選択し、**ポリシー**&#x200B;アイコンをクリックします。 オーサリング用に&#x200B;**レイアウトコンテナ**&#x200B;をロック解除して選択する場合は、**非**&#x200B;に注意してください。

   ![ルートレイアウトコンテナポリシーアイコンを選択します](assets/navigation-routing/root-layout-container-policy.png)

1. **SPA Structure**&#x200B;という名前の新しいポリシーを作成します。

   ![SPA構造ポリシー](assets/navigation-routing/spa-policy-update.png)

   「**許可されているコンポーネント** > **一般** 」で、**レイアウトコンテナ**&#x200B;コンポーネントを選択します。

   **許可されているコンポーネント** > **WKND SPA REACT - STRUCTURE**&#x200B;の下で、**ナビゲーション**&#x200B;コンポーネントを選択します。

   ![ナビゲーションコンポーネントの選択](assets/navigation-routing/select-navigation-component.png)

   **許可されているコンポーネント** > **WKND SPA REACT - Content**&#x200B;の下で、**画像**&#x200B;と&#x200B;**テキスト**&#x200B;コンポーネントを選択します。 合計4つのコンポーネントを選択する必要があります。

   「**完了**」をクリックして、変更を保存します。

1. ページを更新し、ロックされていない&#x200B;**レイアウトコンテナ**&#x200B;の上に&#x200B;**ナビゲーション**&#x200B;コンポーネントを追加します。

   ![テンプレートにナビゲーションコンポーネントを追加](assets/navigation-routing/add-navigation-component.png)

1. **ナビゲーション**&#x200B;コンポーネントを選択し、**ポリシー**&#x200B;アイコンをクリックして、ポリシーを編集します。
1. **ポリシーのタイトル**&#x200B;を&#x200B;**SPA Navigation**&#x200B;にして、新しいポリシーを作成します。

   **プロパティ**&#x200B;の下：

   * **ナビゲーションルート**&#x200B;を`/content/wknd-spa-react/us/en`に設定します。
   * **Exclude Root Levels**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * 「**すべての子ページを収集**」のチェックを外します。
   * **ナビゲーション構造の深さ**&#x200B;を&#x200B;**3**&#x200B;に設定します。

   ![ナビゲーションポリシーの構成](assets/navigation-routing/navigation-policy.png)

   これにより、`/content/wknd-spa-react/us/en`の下の2レベルのナビゲーションが収集されます。

1. 変更を保存すると、テンプレートの一部として入力された`Navigation`が表示されます。

   ![入力済みナビゲーションコンポーネント](assets/navigation-routing/populated-navigation.png)

## 子ページの作成

次に、AEMで別のビューとして機能する追加のページを作成します。 また、AEMが提供するJSONモデルの階層構造も調べます。

1. **サイト**&#x200B;コンソールに移動します。[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). **WKND SPA Reactホームページ**&#x200B;を選択し、**作成** / **ページ**&#x200B;をクリックします。

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

1. 「**テンプレート**」で、「**SPAページ**」を選択します。 「**プロパティ**」に、**タイトル**&#x200B;に&#x200B;**ページ1**&#x200B;を、名前に&#x200B;**ページ1**&#x200B;を入力します。

   ![最初のページのプロパティを入力します](assets/navigation-routing/initial-page-properties.png)

   「**作成**」をクリックし、ダイアログポップアップで「**開く**」をクリックしてAEM SPAエディターでページを開きます。

1. 新しい&#x200B;**テキスト**&#x200B;コンポーネントをメインの&#x200B;**レイアウトコンテナ**&#x200B;に追加します。 コンポーネントを編集し、次のテキストを入力します。**RTEと** H2 **要素を使用して、ページ1**&#x200B;を作成します。

   ![サンプルコンテンツページ1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

1. AEM Sitesコンソールに戻り、上記の手順を繰り返し、**ページ1**&#x200B;の兄弟として&#x200B;**ページ2**&#x200B;という2番目のページを作成します。
1. 最後に、3番目のページ&#x200B;**ページ3**&#x200B;を、**ページ2**&#x200B;の&#x200B;**子**&#x200B;として作成します。 完了すると、サイト階層は次のようになります。

   ![サイト階層のサンプル](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. ナビゲーションコンポーネントを使用して、SPAの様々な領域に移動できるようになりました。

   ![ナビゲーションとルーティング](assets/navigation-routing/navigation-working.gif)

1. AEM Editorの外部でページを開きます。[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). **ナビゲーション**&#x200B;コンポーネントを使用して、アプリの様々なビューに移動します。

1. ブラウザーの開発者ツールを使用して、移動中にネットワークリクエストを調べます。 以下のスクリーンショットは、Google Chromeブラウザーからキャプチャされたものです。

   ![ネットワーク要求の監視](assets/navigation-routing/inspect-network-requests.png)

   最初のページ読み込みの後、以降のナビゲーションでは完全なページ更新がおこなわれず、以前に訪問したページに戻る際にネットワークトラフィックが最小限に抑えられることを確認します。

## 階層ページのJSONモデル {#hierarchy-page-json-model}

次に、SPAのマルチビューエクスペリエンスを推進するJSONモデルを調べます。

1. 新しいタブで、AEMが提供するJSONモデルAPIを開きます。[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). ブラウザー拡張機能を使用してJSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)を[形式設定すると便利です。

   このJSONコンテンツは、SPAが最初に読み込まれたときにリクエストされます。 外側の構造は次のようになります。

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

   `:children`の下に、作成された各ページのエントリが表示されます。 すべてのページのコンテンツは、この最初のJSONリクエストに含まれます。 コンテンツは既にクライアント側で使用可能なので、ナビゲーションルーティングを使用すると、SPAの以降のビューがすばやく読み込まれます。

   最初のJSONリクエストでSPAのコンテンツの&#x200B;**ALL**&#x200B;を読み込むと、最初のページ読み込みが遅くなるので、賢明ではありません。 次に、ページの階層の深さを収集する方法を見てみましょう。

1. 次の場所にある&#x200B;**SPA Root**&#x200B;テンプレートに移動します。[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   **ページプロパティメニュー** / **ページポリシー**&#x200B;をクリックします。

   ![SPA Rootのページポリシーを開きます。](assets/navigation-routing/open-page-policy.png)

1. **SPA Root**&#x200B;テンプレートには、収集されるJSONコンテンツを制御するための追加の「**階層構造**」タブがあります。 **構造の深さ**&#x200B;は、サイト階層の深さを決定し、**ルート**&#x200B;の下の子ページを収集します。 「**構造パターン**」フィールドを使用して、正規表現に基づいて追加のページを除外することもできます。

   **構造の深さ**&#x200B;を&#x200B;**2**&#x200B;に更新します。

   ![構造の深さの更新](assets/navigation-routing/update-structure-depth.png)

   「**完了**」をクリックして、ポリシーに対する変更を保存します。

1. JSONモデル[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を再度開きます。

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

   **ページ3**&#x200B;のパスが削除されていることに注意してください。`/content/wknd-spa-react/us/en/home/page-2/page-3`を最初のJSONモデルから取得します。 これは、**ページ3**&#x200B;が階層のレベル3にあり、レベル2の最大深さのコンテンツのみを含むようにポリシーを更新したからです。

1. SPAホームページを再度開きます。[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)を開き、ブラウザーの開発者ツールを開きます。

   ページを更新すると、SPAルートである`/content/wknd-spa-react/us/en.model.json`へのXHRリクエストが表示されます。 このチュートリアルで既に作成したSPAルートテンプレートの階層の深さ設定に基づいて、3つの子ページのみが含まれています。 **ページ3**&#x200B;は含まれません。

   ![最初のJSONリクエスト — SPA Root](assets/navigation-routing/initial-json-request.png)

1. 開発者ツールを開いた状態で、`Navigation`コンポーネントを使用して&#x200B;**ページ3**&#x200B;に直接移動します。

   新しいXHRリクエストが次の処理に対しておこなわれることを確認します。`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![3ページ目のXHR要求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Managerは、**ページ3**&#x200B;のJSONコンテンツが使用できないことを認識し、追加のXHRリクエストを自動的にトリガーします。

1. 次の場所に直接移動して、ディープリンクを試してみます。[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). また、ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## Inspect React Routing  {#react-routing}

ナビゲーションとルーティングは、[React Router](https://reactrouter.com/)で実装されます。 React Routerは、Reactアプリケーション用のナビゲーションコンポーネントの集まりです。 [AEM Reactコアコンポー](https://github.com/adobe/aem-react-core-wcm-components-base) ネントは、React Routerの機能を使用して、前の手順で使用し **** たナビゲーションコンポーネントを実装します。

次に、React RouterがSPAと統合されている方法を調べ、React Routerの[Link](https://reactrouter.com/web/api/Link)コンポーネントを使用して実験します。

1. IDEで、`ui.frontend/src/index.js`にある`index.js`ファイルを開きます。

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

   `App`は、[React Router](https://reacttraining.com/react-router/)の`Router`コンポーネントでラップされます。 AEM SPA Editor JS SDKが提供する`ModelManager`は、JSONモデルAPIに基づいて、動的ルートをAEMページに追加します。

1. `ui.frontend/src/components/Page/Page.js`にある`Page.js`ファイルを開きます。

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

   `Page` SPAコンポーネントは、`MapTo`関数を使用して、AEMの&#x200B;**Pages**&#x200B;を対応するSPAコンポーネントにマッピングします。 `withRoute`ユーティリティは、`cqPath`プロパティに基づいて、SPAを適切なAEMの子ページに動的にルーティングするのに役立ちます。

1. `ui.frontend/src/components/Header/Header.js`の`Header.js`コンポーネントを開きます。
1. `Header`を更新して、[Link](https://reactrouter.com/web/api/Link)の`<h1>`タグをホームページにラップします。

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

   デフォルトの`<a>`アンカータグを使用する代わりに、React Routerが提供する`<Link>`を使用します。 `to=`が有効なルートを指す限り、SPAはそのルートに切り替わり、**ではなく**、完全なページ更新を実行します。 ここでは、単にホームページへのリンクをハードコード化して、`Link`の使用方法を説明します。

1. `App.test.js`(`ui.frontend/src/App.test.js`)のテストを更新します。

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   `App.js`で参照される静的コンポーネント内でReact Routerの機能を使用しているので、単体テストを更新して対応する必要があります。

1. ターミナルを開き、プロジェクトのルートに移動し、Mavenのスキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEMのSPAのいずれかのページに移動します。[http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   `Navigation`コンポーネントを使用して移動する代わりに、`Header`内のリンクを使用します。

   ![ヘッダーリンク](assets/navigation-routing/header-link.png)

   ページ全体の更新が&#x200B;**トリガーされない**&#x200B;で、SPAルーティングが機能していることを確認します。

1. 必要に応じて、標準の`<a>`アンカータグを使用して`Header.js`ファイルを試します。

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   これは、SPAルーティングと通常のWebページリンクの違いを説明するのに役立ちます。

## おめでとうございます。 {#congratulations}

これで、SPA Editor SDKを使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法を学びました。 React Routerを使用してダイナミックナビゲーションが実装され、`Header`コンポーネントに追加されました。

