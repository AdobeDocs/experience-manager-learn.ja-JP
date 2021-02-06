---
title: 追加ナビゲーションとルーティング | AEM SPA EditorとAngularの使い始めに
description: AEMページとSPAエディタSDKを使用して、SPAで複数の表示をサポートする方法を説明します。 動的なナビゲーションは、Angularルートを使用して実装され、既存のヘッダーコンポーネントに追加されます。
sub-product: サイト
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 2%

---


# 追加ナビゲーションとルーティング{#navigation-routing}

AEMページとSPAエディタSDKを使用して、SPAで複数の表示をサポートする方法を説明します。 動的なナビゲーションは、Angularルートを使用して実装され、既存のヘッダーコンポーネントに追加されます。

## 目的

1. SPAエディタを使用する場合に使用できるSPAモデルルーティングオプションについて説明します。
2. [角度ルーティング](https://angular.io/guide/router)を使用してSPAの異なる表示間を移動する方法を学びます。
3. AEMページ階層によって決まる動的なナビゲーションを実装します。

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
   $ git checkout Angular/navigation-routing-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の[WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)用に完成したパッケージをインストールします。 [WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)から提供された画像は、WKND SPAで再利用されます。 パッケージは、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)に常に表示できます。また、ブランチ`Angular/navigation-routing-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## InspectHeaderComponentの更新{#inspect-header}

前の章では、`HeaderComponent`コンポーネントは、`app.component.html`を介して含まれる純粋なAngularコンポーネントとして追加されました。 この章では、`HeaderComponent`コンポーネントをアプリから削除し、[テンプレートエディター](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)を介して追加します。 これにより、ユーザーはAEM内から`HeaderComponent`のナビゲーションメニューを設定できます。

>[!NOTE]
>
> この章の開始に合わせて、コードベースに対して、CSSとJavaScriptのいくつかの更新が既に行われています。 コア概念に焦点を当てるために、コード変更の&#x200B;**すべて**&#x200B;については説明しません。 [ここ](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start)で、すべての変更を表示できます。

1. 選択したIDEで、この章のSPAスタータープロジェクトを開きます。
2. `ui.frontend`モジュールの下の`header.component.ts`ファイルを検査します。`ui.frontend/src/app/components/header/header.component.ts`。

   `HeaderEditConfig`と`MapTo`を追加し、コンポーネントをAEMコンポーネント`wknd-spa-angular/components/header`にマッピングできるようにしました。

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   `items`の`@Input()`注釈に注意してください。 `items` には、AEMから渡されたナビゲーションオブジェクトの配列が含まれます。

3. `ui.apps`モジュールで、AEM `Header`コンポーネントのコンポーネント定義を調べます。`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header`コンポーネントは、`sling:resourceSuperType`プロパティを介して[ナビゲーションコアコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)のすべての機能を継承します。

## SPAテ追加ンプレート{#add-header-template}のHeaderComponent

1. ブラウザーを開き、AEM [http://localhost:4502/](http://localhost:4502/)にログインします。 開始コードベースは、既に導入されている必要があります。
2. **[!UICONTROL SPAページテンプレート]**&#x200B;に移動します。[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 一番外側の&#x200B;**[!UICONTROL ルートレイアウトコンテナ]**&#x200B;を選択し、**[!UICONTROL ポリシー]**&#x200B;アイコンをクリックします。 オーサリングのために&#x200B;******[!UICONTROL レイアウトコンテナ]**&#x200B;がロック解除されている場合は、注意が必要です。

   ![ルートレイアウトコンテナポリシーアイコンの選択](assets/navigation-routing/root-layout-container-policy.png)

4. 現在のポリシーをコピーし、**[!UICONTROL SPA Structure]**&#x200B;という名前の新しいポリシーを作成します。

   ![SPA構造ポリシー](assets/map-components/spa-policy-update.png)

   **[!UICONTROL 許可されているコンポーネント]** > **[!UICONTROL 一般]**&#x200B;の下で、**[!UICONTROL レイアウトコンテナ]**&#x200B;コンポーネントを選択します。

   **[!UICONTROL 許可されているコンポーネント]** > **[!UICONTROL WKND SPA ANGULAR - STRUCTURE]**&#x200B;の下で、**[!UICONTROL ヘッダー]**&#x200B;コンポーネントを選択します。

   ![ヘッダーコンポーネントを選択](assets/map-components/select-header-component.png)

   **[!UICONTROL 許可されているコンポーネント]** > **[!UICONTROL WKND SPA ANGULAR — コンテンツ]**&#x200B;の下で、**[!UICONTROL 画像]**&#x200B;と&#x200B;**[!UICONTROL テキスト]**&#x200B;コンポーネントを選択します。 合計4つのコンポーネントを選択する必要があります。

   「**[!UICONTROL 完了]**」をクリックして変更を保存します。

5. **ページを更新します。**&lt;追加a0/>ロック解除された&#x200B;**[!UICONTROL レイアウトコンテナ]**&#x200B;の上の&#x200B;**[!UICONTROL ヘッダー]**&#x200B;コンポーネント：

   ![テンプレートにヘッダーコンポーネントを追加](./assets/navigation-routing/add-header-component.gif)

6. **[!UICONTROL ヘッダー]**&#x200B;コンポーネントを選択し、**ポリシー**&#x200B;アイコンをクリックしてポリシーを編集します。

   ![ヘッダーポリシーをクリック](assets/navigation-routing/header-policy-icon.png)

7. **[!UICONTROL ポリシータイトル]**&#x200B;が&#x200B;**&quot;WKND SPAヘッダー&quot;**&#x200B;の新しいポリシーを作成します。

   **[!UICONTROL プロパティ]**&#x200B;の下：

   * **[!UICONTROL ナビゲーションルート]**&#x200B;を`/content/wknd-spa-angular/us/en`に設定します。
   * **[!UICONTROL Exclude Root Levels]**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * 「**[!UICONTROL すべての子ページを収集]**」をオフにします。
   * **[!UICONTROL ナビゲーション構造の深さ]**&#x200B;を&#x200B;**3**&#x200B;に設定します。

   ![ヘッダーポリシーの設定](assets/navigation-routing/header-policy.png)

   これにより、`/content/wknd-spa-angular/us/en`の下のナビゲーション2レベルが収集されます。

8. 変更を保存すると、テンプレートの一部として`Header`が入力されます。

   ![入力されたヘッダーコンポーネント](assets/navigation-routing/populated-header.png)

## 子ページの作成

次に、SPAの別の表示として機能するAEMで追加のページを作成します。 AEMが提供するJSONモデルの階層構造も調査します。

1. **サイト**&#x200B;コンソールに移動します。[http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). **WKND SPA Angularホームページ**&#x200B;を選択し、**[!UICONTROL 作成]**/**[!UICONTROL ページ]**&#x200B;をクリックします。

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

2. 「**[!UICONTROL テンプレート]**」で、「**[!UICONTROL SPAページ]**」を選択します。 「**[!UICONTROL プロパティ]**」に、**「**[!UICONTROL &#x200B;タイトル&#x200B;]**」に「ページ1」**&#x200B;を、名前に&#x200B;**「page-1」**&#x200B;を入力します。

   ![初期ページプロパティの入力](assets/navigation-routing/initial-page-properties.png)

   「**[!UICONTROL 作成]**」をクリックし、ダイアログのポップアップで「**[!UICONTROL 開く]**」をクリックしてAEM SPAエディターでページを開きます。

3. メ追加インの&#x200B;**[!UICONTROL レイアウトコンテナ]**&#x200B;に対する新しい&#x200B;**[!UICONTROL テキスト]**&#x200B;コンポーネント。 コンポーネントを編集し、次のテキストを入力します。**&quot;ページ1&quot;** （RTEと&#x200B;**H1**&#x200B;要素を使用）（段落要素を変更するにはフルスクリーンモードにする必要があります）

   ![サンプルコンテンツページ1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

4. AEM Sitesコンソールに戻り、上記の手順を繰り返して、**&quot;Page 2&quot;**&#x200B;という名前の2番目のページを&#x200B;**Page 1**&#x200B;の兄弟として作成します。 **追加ページ2**&#x200B;の内容を簡単に識別できるように、
5. 最後に、3ページ目&#x200B;**&quot;ページ3&quot;**&#x200B;を&#x200B;**ページ2**&#x200B;の&#x200B;**子**&#x200B;として作成します。 完了したサイト階層は、次のようになります。

   ![サイト階層の例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 新しいタブで、AEMが提供するJSONモデルAPIを開きます。[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). SPAが最初に読み込まれるときに、このJSONコンテンツが要求されます。 外側の構造は次のようになります。

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   `:children`の下に、作成された各ページのエントリが1つずつ表示されます。 すべてのページのコンテンツは、この最初のJSONリクエストに含まれます。 ナビゲーションルーティングが実装されると、コンテンツが既にクライアント側で使用できるので、SPAの以降の表示が迅速に読み込まれます。

   最初のJSONリクエストでSPAのコンテンツの&#x200B;**ALL**&#x200B;を読み込むと、初期のページ読み込みが遅くなるので賢明ではありません。 次に、ページの階層の深さがどのように収集されるかを見てみましょう。

7. 次の場所にある&#x200B;**SPA Root**&#x200B;テンプレートに移動します。[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   **[!UICONTROL ページのプロパティメニュー]**/**[!UICONTROL ページポリシー]**&#x200B;をクリックします。

   ![SPAルートのページポリシーを開く](assets/navigation-routing/open-page-policy.png)

8. **SPAルート**&#x200B;テンプレートには、収集されたJSONコンテンツを制御するための追加の&#x200B;**[!UICONTROL 階層構造]**&#x200B;タブがあります。 **[!UICONTROL 構造の深さ]**&#x200B;は、サイト階層の深さを決定し、**ルート**&#x200B;の下にある子ページを収集します。 また、**[!UICONTROL 構造パターン]**&#x200B;フィールドを使用して、正規式に基づいて追加のページを除外することもできます。

   **[!UICONTROL 構造の深さ]**&#x200B;を&#x200B;**&quot;2&quot;**&#x200B;に更新します。

   ![構造の深さの更新](assets/navigation-routing/update-structure-depth.png)

   「**[!UICONTROL 完了]**」をクリックして、ポリシーに対する変更を保存します。

9. JSONモデル[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)を再度開きます。

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   **ページ3**&#x200B;のパスが削除されたことに注意してください。`/content/wknd-spa-angular/us/en/home/page-2/page-3`を最初のJSONモデルから取得します。

   後で、AEM SPAエディタSDKがどのようにして追加のコンテンツを動的に読み込めるかを確認します。

## ナビゲーションの実装

次に、新しい`NavigationComponent`を使用してナビゲーションメニューを実装します。 `header.component.html`に直接コードを追加することもできますが、大きなコンポーネントを避ける方がよい方法です。 代わりに、後で再使用される可能性のある`NavigationComponent`を実装します。

1. AEM `Header`コンポーネント([http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json))で公開されているJSONを確認します。

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   AEMページの階層性は、ナビゲーションメニューの入力に使用できるJSONでモデル化されます。 `Header`コンポーネントは[ナビゲーションコアコンポーネント](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)のすべての機能を継承し、JSONを通じて公開されたコンテンツは、自動的にAngular `@Input`注釈にマッピングされます。

2. 新しいターミナルウィンドウを開き、SPAプロジェクトの`ui.frontend`フォルダに移動します。 Angular CLIツールを使用して、新しい`NavigationComponent`を作成します。

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 次に、新たに作成した`components/navigation`ディレクトリのAngular CLIを使用して、`NavigationLink`という名前のクラスを作成します。

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 任意のIDEに戻り、`/src/app/components/navigation/navigation-link.ts`の`navigation-link.ts`にあるファイルを開きます。

   ![navigation-link.tsファイルを開きます](assets/navigation-routing/ide-navigation-link-file.png)

5. `navigation-link.ts` に以下を入力します。

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   これは、個々のナビゲーションリンクを表す単純なクラスです。 クラスのコンストラクタでは、`data`はAEMから渡されるJSONオブジェクトと想定しています。 このクラスは、`NavigationComponent`と`HeaderComponent`の両方で使用され、ナビゲーション構造を簡単に設定できます。

   データ変換は実行されません。このクラスは、主にJSONモデルを厳密に入力するために作成されます。 `this.children`は`NavigationLink[]`として型指定され、コンストラクタは`children`配列の各項目に対して新しい`NavigationLink`オブジェクトを再帰的に作成します。 `Header`のJSONモデルは階層的です。

6. `navigation-link.spec.ts` ファイルを開きます。これは`NavigationLink`クラスのテストファイルです。 次の内容に更新します。

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   `const data`は、1つのリンクについて前に検査したのと同じJSONモデルに従います。 これは強力な単体テストとは程遠いが、`NavigationLink`のコンストラクタをテストすれば十分だ。

7. `navigation.component.ts` ファイルを開きます。次の内容に更新します。

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` aemのJSONモデルである `object[]` 名前 `items` のこのクラスは、`NavigationLink`オブジェクトの配列を返す単一のメソッド`get navigationLinks()`を公開します。

8. ファイル`navigation.component.html`を開き、次の内容で更新します。

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   これにより、`<ul>`の最初の値が生成され、`navigation.component.ts`から`get navigationLinks()`メソッドが呼び出されます。 `<ng-container>`は、`recursiveListTmpl`という名前のテンプレートを呼び出し、`navigationLinks`を`links`という名前の変数として渡すために使用します。

   次追加の`recursiveListTmpl`:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   ここでは、ナビゲーションリンクの残りのレンダリングが実装されます。 変数`link`の型は`NavigationLink`で、そのクラスで作成されたすべてのメソッド/プロパティを使用できます。 [`[routerLink]`](https://angular.io/api/router/RouterLink) は、normal `href` 属性の代わりに使用されます。これにより、完全なページ更新を行うことなく、アプリ内の特定のルートにリンクできます。

   また、現在の`link`に空でない`children`配列がある場合は、別の`<ul>`を作成することで、ナビゲーションの再帰部分も実装されます。

9. `navigation.component.spec.ts`を更新して`RouterTestingModule`のサポートを追加：

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   コンポーネントは`[routerLink]`を使用するので、`RouterTestingModule`を追加する必要があります。

10. `navigation.component.scss`を更新して`NavigationComponent`に基本的なスタイルを追加します。

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## ヘッダーコンポーネントの更新

`NavigationComponent`が実装されたので、`HeaderComponent`を更新して参照する必要があります。

1. ターミナルを開き、SPAプロジェクト内の`ui.frontend`フォルダーに移動します。 **webpack dev server**&#x200B;の開始:

   ```shell
   $ npm start
   ```

2. ブラウザーのタブを開き、[http://localhost:4200/](http://localhost:4200/)に移動します。

   **webpack dev server**&#x200B;は、AEMのローカルインスタンス(`ui.frontend/proxy.conf.json`)からJSONモデルをプロキシするように設定する必要があります。 これにより、チュートリアルの前のセクションでAEMで作成したコンテンツに対して直接コードを作成できます。

   ![メニュー切り替え機能](./assets/navigation-routing/nav-toggle-static.gif)

   `HeaderComponent`には現在、メニュー切り替え機能が既に実装されています。 次に、ナビゲーションコンポーネントを追加します。

3. 任意のIDEに戻り、`ui.frontend/src/app/components/header/header.component.ts`の`header.component.ts`ファイルを開きます。
4. `setHomePage()`メソッドを更新して、ハードコードされた文字列を削除し、AEMコンポーネントによって渡された動的なpropを使用します。

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   AEMから渡されるナビゲーションJSONモデルのルート`items[0]`に基づいて`NavigationLink`の新しいインスタンスが作成されます。 `this.route.snapshot.data.path` 現在のAngularルートのパスを返します。この値は、現在のルートが&#x200B;**ホームページ**&#x200B;であるかどうかを判断するために使用されます。 `this.homePageUrl` は、 **ロゴのアンカーリンクを設定するために使用します**。

5. `header.component.html`を開き、ナビゲーション用の静的プレースホルダーを新しく作成した`NavigationComponent`への参照に置き換えます。

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 属性は、 `@Input() items` から、ナビゲーションを構築す `HeaderComponent` る `NavigationComponent` 場所に渡します。

6. `header.component.spec.ts`を開き、`NavigationComponent`の宣言を追加します。

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   `NavigationComponent`は現在`HeaderComponent`の一部として使用されているので、テストベッドの一部として宣言する必要があります。

7. 開いているファイルに変更を保存し、**webpack dev server**&#x200B;に戻ります。[http://localhost:4200/](http://localhost:4200/)

   ![完了したヘッダーナビゲーション](assets/navigation-routing/completed-header.png)

   メニューの切り替えをクリックしてナビゲーションを開くと、表示されたナビゲーションリンクが表示されます。 SPAの別の表示に移動できる必要があります。

## SPAルーティングについて

ナビゲーションが実装されたら、AEMでルーティングを検査します。

1. IDEで、`ui.frontend/src/app`の`app-routing.module.ts`ファイルを開きます。

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   `routes: Routes = [];`配列は、Angularコンポーネントマッピングへのルートまたはナビゲーションパスを定義します。

   `AemPageMatcher` は、このAngularアプリケーションの一部であるAEMのページに「見える」ものに一致する、カスタムのAngularルータ [UrlMatcher](https://angular.io/api/router/UrlMatcher)です。

   `PageComponent` は、AEMのページを表すAngularコンポーネントで、一致したルートが呼び出されます。`PageComponent`はさらに検査されます。

   `AemPageDataResolver`AEM SPA Editor JS SDKが提供するカスタムの [Angular Router ](https://angular.io/api/router/Resolve) Resolverは、AEMのパス（the.html拡張を含む）であるルートURLを、拡張子を下回るページパスであるAEMのリソースパスに変換するために使用されます。

   例えば、`AemPageDataResolver`は、ルートのURL `content/wknd-spa-angular/us/en/home.html`を`/content/wknd-spa-angular/us/en/home`のパスに変換します。 これは、JSONモデルAPI内のパスに基づいてページのコンテンツを解決するために使用されます。

   `AemPageRouteReuseStrategy`AEM SPA Editor JS SDKが提供する、カスタムのRouteReuseStrategyです。これは、ルート [](https://angular.io/api/router/RouteReuseStrategy)  `PageComponent` 間でのルートの再利用を防ぎます。そうしないと、ページ「B」に移動する際に、ページ「A」のコンテンツが表示されることがあります。

2. `page.component.ts`（`ui.frontend/src/app/components/page/`）ファイルを開きます。

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   `PageComponent`は、AEMから取得したJSONを処理するために必要で、ルートをレンダリングするAngularコンポーネントとして使用されます。

   `ActivatedRoute`は、Angular Routerモジュールによって提供され、AEMページのJSONコンテンツをどのページのJSONコンテンツをこのAngular Pageコンポーネントインスタンスにロードするかを示す状態を含みます。

   `ModelManagerService`では、ルートに基づいてJSONデータを取得し、データをクラス変数 `path`、 `items`、にマッピングし `itemsOrder`ます。その後、これらは[AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)に渡されます

3. `ui.frontend/src/app/components/page/`にある`page.component.html`ファイルを開きます

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` には、 [AEMPageComponentが含まれます](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)。変数`path`、`items`、`itemsOrder`が`AEMPageComponent`に渡されます。 SPAエディタJavaScript SDKを介して提供される`AemPageComponent`は、このデータを反復し、[コンポーネントのマップチュートリアル](./map-components.md)に示すように、JSONデータに基づいてAngularコンポーネントを動的にインスタンス化します。

   `PageComponent`は実際には`AEMPageComponent`のプロキシにすぎず、`AEMPageComponent`は重いリフティングの大部分を行い、JSONモデルをAngularコンポーネントに正しくマッピングします。

## AEMでのSPAルーティングのInspect

1. ターミナルを開き、起動した場合は&#x200B;**webpack dev server**&#x200B;を停止します。 プロジェクトのルートに移動し、Mavenスキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Angularプロジェクトでは、非常に厳密なリント規則が有効になっています。 Mavenビルドが失敗した場合は、エラーを確認し、**Lintエラーがリストファイルに見つかったかどうかを確認します。**」を選択します。リンターで見つかった問題を修正し、Mavenコマンドを再実行します。

2. AEMのSPAホームページに移動します。[http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html)を開き、ブラウザーの開発者ツールを開きます。 以下のスクリーンショットは、Google Chromeブラウザーからキャプチャされたものです。

   ページを更新すると、SPAルートである`/content/wknd-spa-angular/us/en.model.json`へのXHRリクエストが表示されます。 チュートリアルの前のバージョンで作成したSPAルートテンプレートの階層深さ設定に基づいて、3つの子ページのみが含まれています。 **ページ3**&#x200B;は含まれません。

   ![初期JSONリクエスト — SPAルート](assets/navigation-routing/initial-json-request.png)

3. 開発者ツールを開き、**ページ3**&#x200B;に移動します。

   ![ページ3のナビゲート](assets/navigation-routing/page-three-navigation.png)

   新しいXHRリクエストが行われた対象は次のとおりです。`/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![ページ3 XHRリクエスト](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Managerは、**Page 3** JSONコンテンツが使用できないことを認識し、追加のXHRリクエストを自動的にトリガーします。

4. 様々なナビゲーションリンクを使用してSPA内を移動し続けます。 追加のXHRリクエストが行われないこと、および完全なページ更新が行われないことを確認します。 これにより、エンドユーザにとってSPAが高速になり、AEMに返される不要な要求が減ります。

   ![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

5. 次の場所に直接移動して、ディープリンクをテストします。[http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## バリデーターが{#congratulations}

SPA Editor SDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を学びました。 動的なナビゲーションは、Angularルーティングを使用して実装され、`Header`コンポーネントに追加されました。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)に常に表示できます。また、ブランチ`Angular/navigation-routing-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

### 次の手順 {#next-steps}

[カスタムコンポーネントの作成](custom-component.md) - AEM SPAエディタで使用するカスタムコンポーネントの作成方法を学びます。JSONモデルを拡張してカスタムコンポーネントに入力するための作成者ダイアログとSlingモデルの開発方法を学びます。
