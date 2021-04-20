---
title: 追加ナビゲーションとルーティング | AEM SPA EditorとReactの使い始めに
description: SPA Editor SDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を説明します。 ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2115'
ht-degree: 3%

---


# 追加ナビゲーションとルーティング{#navigation-routing}

SPA Editor SDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を説明します。 ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。

## 目的

1. SPAエディタを使用する場合に使用できるSPAモデルルーティングオプションについて説明します。
1. [React Router](https://reacttraining.com/react-router/)を使用してSPAの異なる表示間を移動する方法を説明します。
1. AEMページ階層によって決まる動的なナビゲーションを実装します。

## 作成する内容

この章では、既存の`Header`コンポーネントにナビゲーションメニューを追加します。 ナビゲーションメニューはAEMページ階層によって決まり、[Navigation Core Component](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html)が提供するJSONモデルを使用します。

![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 従来の[WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)用に完成したパッケージをインストールします。 [WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)から提供された画像は、WKND SPAで再利用されます。 パッケージは、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)に常に表示できます。また、ブランチ`React/navigation-routing-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## Inspectヘッダーの更新{#inspect-header}

前の章では、`Header`コンポーネントは、`App.js`を介して含まれる純粋なReactコンポーネントとして追加されました。 この章では、`Header`コンポーネントは削除され、[テンプレートエディター](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)を使用して追加されます。 これにより、ユーザーはAEM内から`Header`のナビゲーションメニューを設定できます。

>[!NOTE]
>
> この章の開始に合わせて、コードベースに対して、CSSとJavaScriptのいくつかの更新が既に行われています。 コア概念に焦点を当てるために、コード変更の&#x200B;**すべて**&#x200B;については説明しません。 [ここ](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)で、すべての変更を表示できます。

1. 選択したIDEで、この章のSPAスタータープロジェクトを開きます。
1. `ui.frontend`モジュールの下の`Header.js`ファイルを検査します。`ui.frontend/src/components/Header/Header.js`。

   `HeaderEditConfig`と`MapTo`を追加し、コンポーネントをAEMコンポーネント`wknd-spa-react/components/header`にマッピングできるようにしました。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. `ui.apps`モジュールで、AEM `Header`コンポーネントのコンポーネント定義を調べます。`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   AEM `Header`コンポーネントは、`sling:resourceSuperType`プロパティを介して[ナビゲーションコアコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)のすべての機能を継承します。

## テ追加ンプレートのヘッダ{#add-header-template}

1. ブラウザーを開き、AEM [http://localhost:4502/](http://localhost:4502/)にログインします。 開始コードベースは、既に導入されている必要があります。
1. **SPAページテンプレート**&#x200B;に移動します。[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 一番外側の&#x200B;**ルートレイアウトコンテナ**&#x200B;を選択し、**ポリシー**&#x200B;アイコンをクリックします。 オーサリングのために&#x200B;******レイアウトコンテナ**&#x200B;がロック解除されている場合は、注意が必要です。

   ![ルートレイアウトコンテナポリシーアイコンの選択](assets/navigation-routing/root-layout-container-policy.png)

1. **SPA構造**&#x200B;という名前の新しいポリシーを作成します。

   ![SPA構造ポリシー](assets/navigation-routing/spa-policy-update.png)

   **許可されているコンポーネント** > **一般**&#x200B;の下で、**レイアウトコンテナ**&#x200B;コンポーネントを選択します。

   **許可されているコンポーネント** > **WKND SPA REACT - STRUCTURE**&#x200B;の下で、**ヘッダー**&#x200B;コンポーネントを選択します。

   ![ヘッダーコンポーネントを選択](assets/navigation-routing/select-header-component.png)

   **許可されているコンポーネント** > **WKND SPA REACT — コンテンツ**&#x200B;の下で、**画像**&#x200B;と&#x200B;**テキスト**&#x200B;コンポーネントを選択します。 合計4つのコンポーネントを選択する必要があります。

   「**完了**」をクリックして変更を保存します。

1. ページを更新し、ロック解除された&#x200B;**レイアウトコンテナ**&#x200B;の上に&#x200B;**ヘッダー**&#x200B;コンポーネントを追加します。

   ![テンプレートにヘッダーコンポーネントを追加](./assets/navigation-routing/add-header-component.gif)

1. **ヘッダー**&#x200B;コンポーネントを選択し、**ポリシー**&#x200B;アイコンをクリックしてポリシーを編集します。
1. **ポリシータイトル**&#x200B;が&#x200B;**WKND SPAヘッダー**&#x200B;の新しいポリシーを作成します。

   **プロパティ**&#x200B;の下：

   * **ナビゲーションルート**&#x200B;を`/content/wknd-spa-react/us/en`に設定します。
   * **Exclude Root Levels**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * 「**すべての子ページを収集**」のチェックを外します。
   * **ナビゲーション構造の深さ**&#x200B;を&#x200B;**3**&#x200B;に設定します。

   ![ヘッダーポリシーの設定](assets/navigation-routing/header-policy.png)

   これにより、`/content/wknd-spa-react/us/en`の下のナビゲーション2レベルが収集されます。

1. 変更を保存すると、テンプレートの一部として`Header`が入力されます。

   ![入力されたヘッダーコンポーネント](assets/navigation-routing/populated-header.png)

## 子ページの作成

次に、SPAの別の表示として機能するAEMで追加のページを作成します。 AEMが提供するJSONモデルの階層構造も調査します。

1. **サイト**&#x200B;コンソールに移動します。[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). **WKND SPA Reactホームページ**&#x200B;を選択し、**作成**/**ページ**&#x200B;をクリックします。

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

1. 「**テンプレート**」で、「**SPAページ**」を選択します。 「**プロパティ**」に、****&#x200B;タイトル&#x200B;**にページ1**&#x200B;を、名前に&#x200B;**ページ1**&#x200B;を入力します。

   ![初期ページプロパティの入力](assets/navigation-routing/initial-page-properties.png)

   「**作成**」をクリックし、ダイアログのポップアップで「**開く**」をクリックしてAEM SPAエディターでページを開きます。

1. メ追加インの&#x200B;**レイアウトコンテナ**&#x200B;に対する新しい&#x200B;**テキスト**&#x200B;コンポーネント。 コンポーネントを編集し、次のテキストを入力します。**RTE要素と** H1 **要素を使用したページ1**（段落要素を変更するにはフルスクリーンモードにする必要があります）

   ![サンプルコンテンツページ1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

1. AEM Sitesコンソールに戻り、上記の手順を繰り返して、**ページ2**&#x200B;を&#x200B;**ページ1**&#x200B;の兄弟として作成します。
1. 最後に、3ページ目&#x200B;**ページ3**&#x200B;を&#x200B;**ページ2**&#x200B;の&#x200B;**子**&#x200B;として作成します。 完了したサイト階層は、次のようになります。

   ![サイト階層の例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 新しいタブで、AEMが提供するJSONモデルAPIを開きます。[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). SPAが最初に読み込まれるときに、このJSONコンテンツが要求されます。 外側の構造は次のようになります。

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

   `:children`の下に、作成された各ページのエントリが1つずつ表示されます。 すべてのページのコンテンツは、この最初のJSONリクエストに含まれます。 ナビゲーションルーティングが実装されると、コンテンツが既にクライアント側で使用できるので、SPAの以降の表示が迅速に読み込まれます。

   最初のJSONリクエストでSPAのコンテンツの&#x200B;**ALL**&#x200B;を読み込むと、初期のページ読み込みが遅くなるので賢明ではありません。 次に、ページの階層の深さがどのように収集されるかを見てみましょう。

1. 次の場所にある&#x200B;**SPA Root**&#x200B;テンプレートに移動します。[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   **ページのプロパティメニュー**/**ページポリシー**&#x200B;をクリックします。

   ![SPAルートのページポリシーを開く](assets/navigation-routing/open-page-policy.png)

1. **SPAルート**&#x200B;テンプレートには、収集されたJSONコンテンツを制御するための追加の&#x200B;**階層構造**&#x200B;タブがあります。 **構造の深さ**&#x200B;は、サイト階層の深さを決定し、**ルート**&#x200B;の下にある子ページを収集します。 また、**構造パターン**&#x200B;フィールドを使用して、正規式に基づいて追加のページを除外することもできます。

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

   **ページ3**&#x200B;のパスが削除されたことに注意してください。`/content/wknd-spa-react/us/en/home/page-2/page-3`を最初のJSONモデルから取得します。

   後で、AEM SPAエディタSDKがどのようにして追加のコンテンツを動的に読み込めるかを確認します。

## ナビゲーションの実装

次に、`Header`の一部としてナビゲーションメニューを実装します。 `Header.js`に直接コードを追加することもできますが、大きなコンポーネントを避ける方が良い方法です。 代わりに、後で再利用できる可能性のある`Navigation` SPAコンポーネントを実装します。

1. AEM `Header`コンポーネント([http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json))で公開されているJSONを確認します。

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

   AEMページの階層性は、ナビゲーションメニューの入力に使用できるJSONでモデル化されます。 `Header`コンポーネントは[ナビゲーションコアコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)のすべての機能を継承し、JSONを通じて公開されたコンテンツは自動的にReact propにマッピングされます。

1. 新しいターミナルウィンドウを開き、SPAプロジェクトの`ui.frontend`フォルダに移動します。 **webpack-dev-server**&#x200B;を`npm start`コマンドで開始します。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. 新しいブラウザータブを開き、[http://localhost:3000/](http://localhost:3000/)に移動します。

   **webpack-dev-server**&#x200B;は、AEMのローカルインスタンス(`ui.frontend/.env.development`)からJSONモデルをプロキシするように設定する必要があります。 これにより、前の演習でAEMで作成したコンテンツに対して直接コードを作成できます。 同じブラウジングセッションでAEMに認証されていることを確認します。

   ![メニュー切り替え機能](./assets/navigation-routing/nav-toggle-static.gif)

   `Header`には現在、メニュー切り替え機能が既に実装されています。 次に、ナビゲーションメニューを実装します。

1. 任意のIDEに戻り、`ui.frontend/src/components/Header/Header.js`の`Header.js`を開きます。
1. `homeLink()`メソッドを更新して、ハードコードされた文字列を削除し、AEMコンポーネントによって渡された動的なpropを使用します。

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

   上記のコードは、コンポーネントによって設定されたルートナビゲーション項目に基づいてURLを入力します。 `homeLink()` は、 `logo()` メソッドにロゴを入力するために使用され、に戻るボタンを表示するかどうかを決定するために使用され `backButton()`ます。

   `Header.js`に変更を保存します。

1. `Header.js`の追加上部にある行で、他のインポートの下にある`Navigation`コンポーネントをインポートします。

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. 次に、`get navigation()`メソッドを更新して`Navigation`コンポーネントをインスタンス化します。

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   前述したように、`Header`内にナビゲーションを実装する代わりに、`Navigation`コンポーネント内に大部分のロジックを実装します。  `Header`のpropには、メニューの構築に必要なJSON構造が含まれています。ここでは、すべてのpropを渡します。
1. `Navigation.js`（`ui.frontend/src/components/Navigation/Navigation.js`）ファイルを開きます。
1. `renderGroupNav(children)`メソッドを実装します。

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

   このメソッドは、ナビゲーション項目の配列`children`を受け取り、順番が正しくないリストを作成します。 次に、配列を反復し、`renderNavItem`に項目を渡します。次に実装されます。

1. `renderNavItem`を実装します。

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

   このメソッドは、プロパティ`level`と`active`に基づいてCSSクラスを使用してリスト項目をレンダリングします。 次に、メソッドは`renderLink`を呼び出してアンカータグを作成します。 `Navigation`コンテンツは階層的なので、現在のアイテムの子に対して`renderGroupNav`を呼び出すには再帰的な方法を使用します。

1. `renderLink`メソッドを実装します。

   &lt;a0追加/>Link](https://reacttraining.com/react-router/web/api/Link)コンポーネント（Reactルーターの一部）のインポート方法を、ファイルの最上部に、他のインポートと共に次のようにインポートします。[

   ```js
   import {Link} from "react-router-dom";
   ```

   次に、`renderLink`メソッドの実装を終了します。

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   通常のアンカータグ`<a>`の代わりに、[Link](https://reacttraining.com/react-router/web/api/Link)コンポーネントが使用されます。 これにより、完全なページ更新がトリガされず、代わりにAEM SPA Editor JS SDKが提供するReactルーターが利用されます。

1. `Navigation.js`に変更を保存し、**webpack-dev-server**&#x200B;に戻ります。[http://localhost:3000](http://localhost:3000)

   ![完了したヘッダーナビゲーション](assets/navigation-routing/completed-header.png)

   メニューの切り替えをクリックしてナビゲーションを開くと、表示されたナビゲーションリンクが表示されます。 SPAの別の表示に移動できる必要があります。

## InspectSPAルーティング

ナビゲーションが実装されたら、AEMでルーティングを検査します。

1. IDEで、`ui.frontend/src/index.js`の`index.js`ファイルを開きます。

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

   `App`が[React Router](https://reacttraining.com/react-router/)の`Router`コンポーネントに含まれていることに注意してください。 AEM SPAエディターJS SDKが提供する`ModelManager`は、JSONモデルAPIに基づいて、AEMページに動的ルートを追加します。

1. ターミナルを開き、プロジェクトのルートに移動し、Mavenスキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEMのSPAホームページに移動します。[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)を開き、ブラウザーの開発者ツールを開きます。 以下のスクリーンショットは、Google Chromeブラウザーからキャプチャされたものです。

   ページを更新すると、SPAルートである`/content/wknd-spa-react/us/en.model.json`へのXHRリクエストが表示されます。 チュートリアルの前のバージョンで作成したSPAルートテンプレートの階層深さ設定に基づいて、3つの子ページのみが含まれています。 **ページ3**&#x200B;は含まれません。

   ![初期JSONリクエスト — SPAルート](assets/navigation-routing/initial-json-request.png)

1. 開発者ツールを開いた状態で、`Header`ナビゲーションを使用して&#x200B;**ページ3**&#x200B;に移動します。

   ![ページ3のナビゲート](assets/navigation-routing/page-three-navigation.png)

   新しいXHRリクエストが行われた対象は次のとおりです。`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![ページ3 XHRリクエスト](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Managerは、**Page 3** JSONコンテンツが使用できないことを認識し、追加のXHRリクエストを自動的にトリガーします。

1. `Header`コンポーネントの様々なナビゲーションリンクを使用してSPA内を移動し続けます。 追加のXHRリクエストが行われないこと、および完全なページ更新が行われないことを確認します。 これにより、エンドユーザにとってSPAが高速になり、AEMに返される不要な要求が減ります。

   ![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

1. 次の場所に直接移動して、ディープリンクをテストします。[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## これで完了です! {#congratulations}

SPA Editor SDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を学びました。 ダイナミックナビゲーションは、React Routerを使用して実装され、`Header`コンポーネントに追加されました。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)に常に表示できます。また、ブランチ`React/navigation-routing-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。
