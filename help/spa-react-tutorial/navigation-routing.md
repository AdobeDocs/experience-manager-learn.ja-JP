---
title: 追加ナビゲーションとルーティング | AEM SPAエディターとReactの使い始めに
description: SPAエディターSDKを使用してAEMページにマッピングすることで、SPA内の複数の表示をサポートする方法を説明します。 ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。
sub-product: サイト
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 3%

---


# 追加ナビゲーションとルーティング {#navigation-routing}

SPAエディターSDKを使用してAEMページにマッピングすることで、SPA内の複数の表示をサポートする方法を説明します。 ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。

## 目的

1. SPAエディタを使用する場合に使用できるSPAモデルのルーティングオプションを理解します。
2. React Routerを使用してSPAの異なる表示間を移動する方法を説明し [ます](https://reacttraining.com/react-router/) 。
3. AEMページ階層によって決まる動的なナビゲーションを実装します。

## 作成する内容

この章では、既存のコンポー `Header` ネントにナビゲーションメニューを追加します。 ナビゲーションメニューはAEMページ階層によって決まり、 [Navigation Coreコンポーネントが提供するJSONモデルを使用します](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html)。

![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の [WKNDリファレンスサイト用に完成したパッケージをインストールし](https://github.com/adobe/aem-guides-wknd/releases/latest)ます。 WKNDリファレンスサイトから提供される [画像は](https://github.com/adobe/aem-guides-wknd/releases/latest) 、WKND SPAで再利用されます。 パッケージは、 [AEM Package Managerを使用してインストールできます](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/navigation-routing-solution`ます。

## Inspectヘッダーの更新 {#inspect-header}

前の章では、 `Header` コンポーネントは、を介して含まれる純粋なReactコンポーネントとして追加され `App.js`ました。 この章では、 `Header` コンポーネントは削除され、 [テンプレートエディタを使用して追加されます](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)。 これにより、ユーザーはAEM内 `Header` からののナビゲーションメニューを設定できます。

>[!NOTE]
>
> この章の開始に合わせて、コードベースに対して、CSSとJavaScriptのいくつかの更新が既に行われています。 コアの概念に焦点を当てるために、コードの変更 **の一部については説明しません** 。 ここでは、すべての変更を表示で [きます](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)。

1. 選択したIDEで、この章のSPAスタータープロジェクトを開きます。
2. モジュールの下で、次の場所にあるファイル `ui.frontend``Header.js` を検査します。 `ui.frontend/src/components/Header/Header.js`.

   コンポーネントをAEMコンポーネントにマッピングできるよう `HeaderEditConfig` に、と `MapTo` を追加するなど、いくつかの更新が行われました `wknd-spa-react/components/header`。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

3. モジュールで、AEMコンポーネントのコンポーネント定義を `ui.apps` 検査し `Header` ます。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   AEM `Header` コンポーネントは、プ [ロパティを介して、](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html)`sling:resourceSuperType` Navigation Coreコンポーネントのすべての機能を継承します。

## テ追加ンプレートのヘッダー {#add-header-template}

1. ブラウザーを開き、AEM(http://localhost:4502/ [)にログインします](http://localhost:4502/)。 開始コードベースは、既に導入されている必要があります。
2. Navigate to the **SPA Page Template**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Select the outer-most **Root Layout Container** and click its **Policy** icon. オーサリングのた **めに** 、 **レイアウトコンテナ** （ロック解除）を選択しないように注意してください。

   ![ルートレイアウトコンテナポリシーアイコンの選択](assets/navigation-routing/root-layout-container-policy.png)

4. 「 **SPA Structure**」という名前の新しいポリシーを作成します。

   ![SPA構造ポリシー](assets/navigation-routing/spa-policy-update.png)

   「 **許可されているコンポーネント** 」 **>「** 一般」 **>「** レイアウトコンテナ」コンポーネントを選択します。

   「 **許可されているコンポーネント** / **WKND SPA REACT — 構造** 」で、「 **ヘッダー** コンポーネント」を選択します。

   ![ヘッダーコンポーネントを選択](assets/navigation-routing/select-header-component.png)

   「 **Allowed Components** 」 **>「** WKND SPA REACT - Content **」で、「Image** Components」と「 **** Text Components」を選択します。 合計4つのコンポーネントを選択する必要があります。

   Click **Done** to save the changes.

5. ページを更新し、ロックされていない **レイアウトコンテナの上に** Header **コンポーネントを追加します**。

   ![テンプレートにヘッダーコンポーネントを追加](./assets/navigation-routing/add-header-component.gif)

6. 「 **Header** 」コンポーネントを選択し、「 **Policy** 」アイコンをクリックしてポリシーを編集します。
7. WKND SPAヘッダーの **ポリシータイトル** を使用して新しいポリシーを作成します ****。

   「 **プロパティ**」で、

   * 「 **ナビゲーションルート** 」をに設定し `/content/wknd-spa-react/us/en`ます。
   * 「ルートレベルを **除外」を** 1に設定し **ます**。
   * Uncheck **Collect all child pages**.
   * 「 **ナビゲーション構造の深さ** 」を「 **3**」に設定します。

   ![ヘッダーポリシーの設定](assets/navigation-routing/header-policy.png)

   これにより、深さ2レベルのナビゲーションが収集され `/content/wknd-spa-react/us/en`ます。

8. 変更を保存すると、テンプレートの一部として次のよう `Header` に入力された内容が表示されます。

   ![入力されたヘッダーコンポーネント](assets/navigation-routing/populated-header.png)

## 子ページの作成

次に、AEMで、SPA内の異なる表示として機能する追加のページを作成します。 AEMが提供するJSONモデルの階層構造も調査します。

1. サイト **コンソールに移動します** 。 [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 「 **WKND SPA React」ホームページを選択し** 、 **作成** / **ページ**&#x200B;をクリックします。

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

2. 「 **テンプレート** 」で「 **SPAページ**」を選択します。 「 **プロパティ** 」の下で、「タイトル **」に「Page 1** 」と入力し、「 **ページ1」に名前を****** 入力します。

   ![初期ページプロパティの入力](assets/navigation-routing/initial-page-properties.png)

   「 **作成** 」をクリックし、ダイアログのポップアップで「 **開く** 」をクリックして、AEM SPAエディタでページを開きます。

3. メインの **レイアウトコンテナに対する新しい** 「 **テキスト**」コンポーネント。 コンポーネントを編集し、次のテキストを入力します。 **ページ1** (RTEと **H1** 要素を使用)

   ![サンプルコンテンツページ1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

4. AEM Sitesコンソールに戻り、上記の手順を繰り返して、 **Page 2** という名前の2番目のページを **Page 1の兄弟として作成します**。
5. 最後に、3ページ目の **ページ** 「3 **ページ目** 」を2 ****&#x200B;ページ目の子として作成します。 完了したサイト階層は、次のようになります。

   ![サイト階層の例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 新しいタブで、AEMが提供するJSONモデルAPIを開きます。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). SPAが最初に読み込まれるときに、このJSONコンテンツが要求されます。 外側の構造は次のようになります。

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

   下 `:children` に、作成された各ページのエントリが表示されます。 すべてのページのコンテンツは、この最初のJSONリクエストに含まれます。 ナビゲーションルーティングが実装されると、そのコンテンツが既にクライアント側で使用可能なので、以降の表示のSPAは迅速に読み込まれます。

   最初のJSONリクエストでSPAのコンテンツの **ALL** （すべて）を読み込むと、初期ページの読み込みが遅くなるので賢明ではありません。 次に、ページの階層の深さがどのように収集されるかを見てみましょう。

7. 次の場所にある **SPAルート** ・テンプレートに移動します。 [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   ページのプロパティメニュー **/** ページポリシー ****:

   ![SPAルートのページポリシーを開く](assets/navigation-routing/open-page-policy.png)

8. 「 **SPAルート** 」テンプレートには、収集されたJSONコンテンツを制御する「 **階層構造** 」タブが追加されています。 構 **造の深さ** は、サイト階層の深さを決定し、ルートの下に子ページを収集し **ます**。 また、「 **構造パターン** 」フィールドを使用して、正規式に基づいて追加のページをフィルターで除外することもできます。

   構 **造の深さ** を **2に更新します**。

   ![構造の深さの更新](assets/navigation-routing/update-structure-depth.png)

   「 **完了** 」をクリックして、ポリシーに対する変更を保存します。

9. JSONモデルhttp://localhost:4502/content/wknd-spa-react/us/en.model.jsonを再度開き [ます](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

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

   「 **ページ3** 」のパスが削除されていることに注意してください。 `/content/wknd-spa-react/us/en/home/page-2/page-3` を最初のJSONモデルから取得します。

   後で、AEM SPAエディターSDKがどのようにして追加コンテンツを動的に読み込めるかを確認します。

## ナビゲーションの実装

次に、の一部としてナビゲーションメニューを実装し `Header`ます。 コードを直接追加することもできますが、大きなコンポーネントを避け `Header.js` る方が効果的です。 代わりに、後で再利用できる可能性のある `Navigation` SPAコンポーネントを実装します。

1. AEMコンポーネントによって公開されているJSONをhttp://localhost:4502/content/wknd-spa-react/us/en.model.jsonで確認し `Header` ます [](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   AEMページの階層性は、ナビゲーションメニューの入力に使用できるJSONでモデル化されます。 コンポーネントは `Header`[](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html) Navigation Coreコンポーネントのすべての機能を継承し、JSONを通じて公開されたコンテンツは自動的にReact propにマッピングされることに留意してください。

2. 新しいターミナルウィンドウを開き、SPAプロジェクトの `ui.frontend` フォルダに移動します。 webpack- **dev-server** にコマンドを開始 `npm start`します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

3. Open a new browser tab and navigate to [http://localhost:3000/](http://localhost:3000/).

   webpack- **dev-server** は、AEMのローカルインスタンス(`ui.frontend/.env.development`)からJSONモデルをプロキシするように設定する必要があります。 これにより、前の演習でAEMで作成したコンテンツに対して直接コードを作成できます。 同じブラウジングセッションでAEMに認証されていることを確認します。

   ![メニュー切り替え機能](./assets/navigation-routing/nav-toggle-static.gif)

   には、 `Header` 現在、メニュー切り替え機能が既に実装されています。 次に、ナビゲーションメニューを実装します。

4. 選択したIDEに戻り、atを開き `Header.js` ま `ui.frontend/src/components/Header/Header.js`す。
5. この `homeLink()` メソッドを更新して、ハードコードされた文字列を削除し、AEMコンポーネントによって渡された動的なpropを使用します。

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   上記のコードは、コンポーネントによって設定されたルートナビゲーション項目に基づいてURLを入力します。 `homeLink()` は、メソッドにロゴを入力するために使用され、に戻るボタンを表示するかどうかを決定するために使用され `logo()``backButton()`ます。

   に変更を保存し `Header.js`ます。

6. 上部にある線追加で、他の読み込みの下にあるコンポー `Header.js``Navigation` ネントを読み込みます。

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

7. 次に、この `get navigation()` メソッドを更新して、コンポー `Navigation` ネントをインスタンス化します。

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   前述したように、アドビでは、内部にナビゲーションを実装する代わりに、コンポー `Header``Navigation` ネント内の大部分のロジックを実装します。  のpropには、メニューの構築に必要なJSON構造が `Header` 含まれているので、すべてのpropを渡します。
8. Open the file `Navigation.js` at `ui.frontend/src/components/Navigation/Navigation.js`.
9. メソッドの実装 `renderGroupNav(children)` :

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   このメソッドは、ナビゲーション項目の配列を受け取り `children`、順不同のリストを作成します。 その後、アレイを反復し、にアイテムを渡します。こ `renderNavItem`のアイテムは次に実装されます。

10. 以下を実装し `renderNavItem`ます。

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   このメソッドは、プロパティとに基づいてCSSクラスを使用してリスト項目をレンダリング `level` し `active`ます。 その後、メソッドがアンカータグ `renderLink` の作成を呼び出します。 コンテンツは階層的な `Navigation` ので、再帰的な方法を使用して、現在のアイテムの子 `renderGroupNav` のを呼び出します。

11. メソッドの実装 `renderLink` :

   ファイルの追加最上部にある [Link](https://reacttraining.com/react-router/web/api/Link) コンポーネント（Reactルータの一部）のインポート方法と、他のインポート方法を次に示します。

   ```js
   import {Link} from "react-router-dom";
   ```

   次に、この `renderLink` メソッドの実装を完了します。

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   通常のアンカータグの代わりに、 `<a>`Link [](https://reacttraining.com/react-router/web/api/Link) コンポーネントが使用されます。 これにより、完全なページ更新がトリガされず、代わりにAEM SPA Editor JS SDKが提供するReactルーターが使用されます。

12. 変更を `Navigation.js` webpack-dev-server **に保存し、** webpack-dev-serverに戻します。 [http://localhost:3000](http://localhost:3000)

   ![完了したヘッダーナビゲーション](assets/navigation-routing/completed-header.png)

   メニューの切り替えをクリックしてナビゲーションを開くと、表示されたナビゲーションリンクが表示されます。 別の表示のSPAに移動できる必要があります。

## Inspectザスパルーティング

ナビゲーションが実装されたら、AEMでルーティングを検査します。

1. IDEで、次の場所にあるファイル `index.js` を開き `ui.frontend/src/index.js`ます。

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

   が、 `App` React Routerのコンポーネントに含まれていることに注意してください `Router`[](https://reacttraining.com/react-router/)。 AEM SPA Editor JS SDKが提供 `ModelManager`するJSONモデルAPIに基づいて、動的ルートをAEMページに追加します。

2. ターミナルを開き、プロジェクトのルートに移動し、Mavenスキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. AEMのSPAホームページに移動します。 [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 、ブラウザーの開発者ツールを開きます。 以下のスクリーンショットは、Google Chromeブラウザーからキャプチャされたものです。

   ページを更新すると、SPAルート `/content/wknd-spa-react/us/en.model.json`であるXHRリクエストが表示されます。 チュートリアルで前述したSPAルートテンプレートに対する階層の深さの設定に基づいて、3つの子ページのみが含まれることに注意してください。 これには **ページ3は含まれません**。

   ![初期JSONリクエスト — SPAルート](assets/navigation-routing/initial-json-request.png)

4. 開発者ツールを開いた状態で、 `Header` ナビゲーションを使用して **3ページに移動します**。

   ![ページ3のナビゲート](assets/navigation-routing/page-three-navigation.png)

   新しいXHRリクエストが行われた対象は次のとおりです。 `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![ページ3 XHRリクエスト](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Managerは、 **ページ3** JSONコンテンツが使用できないことを認識し、追加のXHRリクエストを自動的にトリガーします。

5. コンポーネントの様々なナビゲーションリンクを使用して、SPAのナビゲーションを続け `Header` ます。 追加のXHRリクエストが行われないこと、および完全なページ更新が行われないことを確認します。 これにより、SPAがエンドユーザーに対して高速になり、AEMに返される不要な要求が減ります。

   ![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

6. 次の場所に直接移動して、ディープリンクをテストします。 [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## バリデーターが{#congratulations}

SPAエディタSDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を学びました。 ダイナミックナビゲーションは、React Routerを使用して実装され、コンポー `Header` ネントに追加されました。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/navigation-routing-solution`ます。
