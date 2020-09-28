---
title: 追加ナビゲーションとルーティング | AEM SPAエディターとAngularの使用の手引き
description: AEMページとSPAエディターSDKを使用して、SPA内の複数の表示をサポートする方法を説明します。 動的なナビゲーションは、Angularルートを使用して実装され、既存のヘッダーコンポーネントに追加されます。
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
ht-degree: 1%

---


# 追加ナビゲーションとルーティング {#navigation-routing}

AEMページとSPAエディターSDKを使用して、SPA内の複数の表示をサポートする方法を説明します。 動的なナビゲーションは、Angularルートを使用して実装され、既存のヘッダーコンポーネントに追加されます。

## 目的

1. SPAエディタを使用する場合に使用できるSPAモデルのルーティングオプションを理解します。
2. SPAの異なる表示間を移動する [角度ルーティング](https://angular.io/guide/router) (Angular Angular Navigator)の使用方法を説明します。
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
   $ git checkout Angular/navigation-routing-start
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

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/navigation-routing-solution`ます。

## InspectHeaderComponentの更新 {#inspect-header}

前の章では、 `HeaderComponent` コンポーネントは、を介して含まれる純粋なAngularコンポーネントとして追加され `app.component.html`ました。 この章では、コンポー `HeaderComponent` ネントがアプリから削除され、 [テンプレートエディターを使用して追加されます](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)。 これにより、ユーザーはAEM内 `HeaderComponent` からののナビゲーションメニューを設定できます。

>[!NOTE]
>
> この章の開始に合わせて、コードベースに対して、CSSとJavaScriptのいくつかの更新が既に行われています。 コアの概念に焦点を当てるために、コードの変更 **の一部については説明しません** 。 ここでは、すべての変更を表示で [きます](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start)。

1. 選択したIDEで、この章のSPAスタータープロジェクトを開きます。
2. モジュールの下で、次の場所にあるファイル `ui.frontend``header.component.ts` を検査します。 `ui.frontend/src/app/components/header/header.component.ts`.

   コンポーネントをAEMコンポーネントにマッピングできるよう `HeaderEditConfig` に、と `MapTo` を追加するなど、いくつかの更新が行われました `wknd-spa-angular/components/header`。

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

   の `@Input()` 注釈をメモしておき `items`ます。 `items` には、AEMから渡されたナビゲーションオブジェクトの配列が含まれます。

3. モジュールで、AEMコンポーネントのコンポーネント定義を `ui.apps` 検査し `Header` ます。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` コンポーネントは、プ [ロパティを介して、](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html)`sling:resourceSuperType` Navigation Coreコンポーネントのすべての機能を継承します。

## SPAテ追加ンプレートのHeaderComponent {#add-header-template}

1. ブラウザーを開き、AEM(http://localhost:4502/ [)にログインします](http://localhost:4502/)。 開始コードベースは、既に導入されている必要があります。
2. Navigate to the **[!UICONTROL SPA Page Template]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Select the outer-most **[!UICONTROL Root Layout Container]** and click its **[!UICONTROL Policy]** icon. オーサリングのた **めに** 、 **[!UICONTROL レイアウトコンテナ]** （ロック解除）を選択しないように注意してください。

   ![ルートレイアウトコンテナポリシーアイコンの選択](assets/navigation-routing/root-layout-container-policy.png)

4. 現在のポリシーをコピーし、「 **[!UICONTROL SPA Structure]**」という名前の新しいポリシーを作成します。

   ![SPA構造ポリシー](assets/map-components/spa-policy-update.png)

   「 **[!UICONTROL 許可されているコンポーネント]** 」 **[!UICONTROL >「]** 一般」 **[!UICONTROL >「]** レイアウトコンテナ」コンポーネントを選択します。

   「 **[!UICONTROL 許可されているコンポーネント]** 」( **[!UICONTROL Allowed Components]** ) > 「WKND SPA ANGULAR - STRUCTURE **[!UICONTROL 」(WKND SPA ANGULAR - STRUCTURE]** )の下で、「ヘッダー」(Header)コンポーネントを選択します。

   ![ヘッダーコンポーネントを選択](assets/map-components/select-header-component.png)

   「 **[!UICONTROL 許可されているコンポーネント]** 」>「 **[!UICONTROL WKND SPA ANGULAR - Content]** 」の下で、「 **[!UICONTROL Image]** Components」と「 **** Text Components」を選択します。 合計4つのコンポーネントを選択する必要があります。

   Click **[!UICONTROL Done]** to save the changes.

5. **ページを更新します。**&#x200B;ロックされ追加ていない **[!UICONTROL レイアウトコンテナの上にある「]** Header **[!UICONTROL 」コンポーネント]**:

   ![テンプレートにヘッダーコンポーネントを追加](./assets/navigation-routing/add-header-component.gif)

6. 「 **[!UICONTROL Header]** 」コンポーネントを選択し、「 **Policy** 」アイコンをクリックしてポリシーを編集します。

   ![ヘッダーポリシーをクリック](assets/navigation-routing/header-policy-icon.png)

7. 「WKND SPA Header」の **[!UICONTROL ポリシータイトルを持つ新しい]** ポリシーを作成し ****&#x200B;ます。

   「 **[!UICONTROL プロパティ]**」で、

   * 「 **[!UICONTROL ナビゲーションルート]** 」をに設定し `/content/wknd-spa-angular/us/en`ます。
   * 「ルートレベルを **[!UICONTROL 除外」を]** 1に設定し **ます**。
   * 「すべての子ページを **[!UICONTROL 収集]**」をオフにします。
   * 「 **[!UICONTROL ナビゲーション構造の深さ]** 」を「 **3**」に設定します。

   ![ヘッダーポリシーの設定](assets/navigation-routing/header-policy.png)

   これにより、深さ2レベルのナビゲーションが収集され `/content/wknd-spa-angular/us/en`ます。

8. 変更を保存すると、テンプレートの一部として次のよう `Header` に入力された内容が表示されます。

   ![入力されたヘッダーコンポーネント](assets/navigation-routing/populated-header.png)

## 子ページの作成

次に、AEMで、SPA内の異なる表示として機能する追加のページを作成します。 AEMが提供するJSONモデルの階層構造も調査します。

1. サイト **コンソールに移動します** 。 [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). 「 **WKND SPA角度ホームページ** 」( **[!UICONTROL WKND SPA Angular)を選択し]** 、 **[!UICONTROL 作成]**/ページをクリックします。

   ![ページを新規作成](assets/navigation-routing/create-new-page.png)

2. 「 **[!UICONTROL テンプレート]** 」で「 **[!UICONTROL SPAページ]**」を選択します。 「 **[!UICONTROL プロパティ]** 」の下で、「 **タイトル」に「ページ1」** と入力し、「 **[!UICONTROL ページ1]** 」と入力して名前を入力し **** ます。

   ![初期ページプロパティの入力](assets/navigation-routing/initial-page-properties.png)

   「 **[!UICONTROL 作成]** 」をクリックし、ダイアログのポップアップで「 **[!UICONTROL 開く]** 」をクリックして、AEM SPAエディタでページを開きます。

3. メインの **[!UICONTROL レイアウトコンテナに対する新しい]** 「 **[!UICONTROL テキスト]**」コンポーネント。 コンポーネントを編集し、次のテキストを入力します。 **RTEと** H1 **要素を使用した「ページ1」** （段落要素を変更するにはフルスクリーンモードにする必要があります）

   ![サンプルコンテンツページ1](assets/navigation-routing/page-1-sample-content.png)

   画像などのコンテンツを自由に追加できます。

4. AEM Sitesコンソールに戻り、上記の手順を繰り返して、「Page 2」という名前の2番目のページを **ページ1** の兄弟として作成します ****。 追加2 **** ページ目のコンテンツを参照してください。
5. 最後に、3ページ目 **「ページ3」** を2 **ページ目の子として作成し******&#x200B;ます。 完了したサイト階層は、次のようになります。

   ![サイト階層の例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 新しいタブで、AEMが提供するJSONモデルAPIを開きます。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). SPAが最初に読み込まれるときに、このJSONコンテンツが要求されます。 外側の構造は次のようになります。

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

   下 `:children` に、作成された各ページのエントリが表示されます。 すべてのページのコンテンツは、この最初のJSONリクエストに含まれます。 ナビゲーションルーティングが実装されると、そのコンテンツが既にクライアント側で使用可能なので、以降の表示のSPAは迅速に読み込まれます。

   最初のJSONリクエストでSPAのコンテンツの **ALL** （すべて）を読み込むと、初期ページの読み込みが遅くなるので賢明ではありません。 次に、ページの階層の深さがどのように収集されるかを見てみましょう。

7. 次の場所にある **SPAルート** ・テンプレートに移動します。 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   ページのプロパティメニュー **[!UICONTROL /]** ページポリシー ****:

   ![SPAルートのページポリシーを開く](assets/navigation-routing/open-page-policy.png)

8. 「 **SPAルート** 」テンプレートには、収集されたJSONコンテンツを制御する「 **[!UICONTROL 階層構造]** 」タブが追加されています。 構 **[!UICONTROL 造の深さ]** は、サイト階層の深さを決定し、ルートの下に子ページを収集し **ます**。 また、「 **[!UICONTROL 構造パターン]** 」フィールドを使用して、正規式に基づいて追加のページをフィルターで除外することもできます。

   構 **[!UICONTROL 造の深さ]** を「2」に更新 ****:

   ![構造の深さの更新](assets/navigation-routing/update-structure-depth.png)

   「 **[!UICONTROL 完了]** 」をクリックして、ポリシーに対する変更を保存します。

9. JSONモデルhttp://localhost:4502/content/wknd-spa-angular/us/en.model.jsonを再度開き [ます](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

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

   「 **ページ3** 」のパスが削除されていることに注意してください。 `/content/wknd-spa-angular/us/en/home/page-2/page-3` を最初のJSONモデルから取得します。

   後で、AEM SPAエディターSDKがどのようにして追加コンテンツを動的に読み込めるかを確認します。

## ナビゲーションの実装

次に、新しいナビゲーションメニューを導入し `NavigationComponent`ます。 コードを直接に追加することもできますが、大きなコンポーネント `header.component.html` を避ける方が効果的です。 その代わりに、後で再利用でき `NavigationComponent` る可能性のあるを実装します。

1. AEMコンポーネントによって公開されているJSONをhttp://localhost:4502/content/wknd-spa-angular/us/en.model.jsonで確認し `Header` ます [](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

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

   AEMページの階層性は、ナビゲーションメニューの入力に使用できるJSONでモデル化されます。 コンポーネントは、 `Header` ナビゲーションコアコンポーネントのすべての機能を継承し [、JSONを通じて公開されたコンテンツは、自動的にAngular](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)`@Input` 注釈にマッピングされます。

2. 新しいターミナルウィンドウを開き、SPAプロジェクトの `ui.frontend` フォルダに移動します。 Angular CLIツールを `NavigationComponent` 使用して、新しいを作成します。

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 次に、新たに作成したディレクトリ `NavigationLink` にAngular CLIを使用して、という名前のクラスを作成し `components/navigation` ます。

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 選択したIDEに戻り、のファイルを開き `navigation-link.ts` ま `/src/app/components/navigation/navigation-link.ts`す。

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

   これは、個々のナビゲーションリンクを表す単純なクラスです。 クラスのコンストラクターでは、AEMから渡 `data` されるJSONオブジェクトと想定します。 このクラスは、との両方で使用され、ナビゲーション構造を簡単 `NavigationComponent``HeaderComponent` に設定できます。

   データ変換は実行されません。このクラスは、主にJSONモデルを厳密に入力するために作成されます。 にを指定 `this.children` し、コンストラクタは `NavigationLink[]` 配列内の各項目に対して新しい `NavigationLink``children` オブジェクトを再帰的に作成します。 のJSONモデルは階層的 `Header` です。

6. Open the file `navigation-link.spec.ts`. これは、 `NavigationLink` クラスのテストファイルです。 次の内容に更新します。

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

   これは、前に検査したのと同じJSONモデルに `const data` 従って、単一のリンクに対して行われます。 これは、堅牢な単体テストとは程遠いですが、のコンストラクタをテストするのに十分で `NavigationLink`す。

7. Open the file `navigation.component.ts`. 次の内容に更新します。

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

   `NavigationComponent` aemのJSONモデルで `object[]` ある `items` 名前の付いたが、 このクラスは、オブジェクトの配列 `get navigationLinks()` を返す単一のメソッドを公開し `NavigationLink` ます。

8. ファイルを開き、次の内容 `navigation.component.html` で更新します。

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   これにより、イニシャルが生成 `<ul>` され、から `get navigationLinks()` メソッドが呼び出され `navigation.component.ts`ます。 は、という名前 `<ng-container>` のテンプレートの呼び出しを行い `recursiveListTmpl` 、をという名前の変数 `navigationLinks` として渡すのに使用され `links`ます。

   次追加: `recursiveListTmpl`

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

   ここでは、ナビゲーションリンクの残りのレンダリングが実装されます。 変数の型 `link` はで、そのクラスで作成されたすべてのメソッド `NavigationLink` /プロパティが使用可能です。 [`[routerLink]`](https://angular.io/api/router/RouterLink) は、通常の `href` 属性の代わりに使用されます。 これにより、完全なページ更新を行うことなく、アプリ内の特定のルートにリンクできます。

   また、現在の `<ul>` 配列が空でない場合は、別の配列を作成することで、ナビゲーションの再帰部分 `link``children` も実装されます。

9. 次のサポート `navigation.component.spec.ts` を追加するために更新 `RouterTestingModule`:

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

   コンポーネント `RouterTestingModule` はを使用するので、を追加する必要があり `[routerLink]`ます。

10. を更新 `navigation.component.scss` し、次に基本的なスタイルを追加し `NavigationComponent`ます。

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

が実装され `NavigationComponent` たので、を更新して参照する必要 `HeaderComponent` があります。

1. ターミナルを開き、SPAプロジェクト内の `ui.frontend` フォルダに移動します。 Webpack Devサーバーの **開始**:

   ```shell
   $ npm start
   ```

2. Open a browser tab and navigate to [http://localhost:4200/](http://localhost:4200/).

   WebPack **開発サーバー** は、AEM(`ui.frontend/proxy.conf.json`)のローカルインスタンスからJSONモデルをプロキシするように設定する必要があります。 これにより、チュートリアルの前のセクションでAEMで作成したコンテンツに対して直接コードを作成できます。

   ![メニュー切り替え機能](./assets/navigation-routing/nav-toggle-static.gif)

   には、 `HeaderComponent` 現在、メニュー切り替え機能が既に実装されています。 次に、ナビゲーションコンポーネントを追加します。

3. 選択したIDEに戻り、次の場所でファイルを開き `header.component.ts` ま `ui.frontend/src/app/components/header/header.component.ts`す。
4. この `setHomePage()` メソッドを更新して、ハードコードされた文字列を削除し、AEMコンポーネントによって渡された動的なpropを使用します。

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

   の新しいインスタンス `NavigationLink` が、AEMから渡されたナビゲーションJSONモデルのルートに基づいて作成され `items[0]`ます。 `this.route.snapshot.data.path` 現在のAngularルートのパスを返します。 この値は、現在のルートが **ホームページであるかどうかを判断するために使用されます**。 `this.homePageUrl` は、 **ロゴのアンカーリンクを設定するために使用します**。

5. ナビゲーション用の静的プレースホルダ `header.component.html` ーを開き、新しく作成した静的プレースホルダーへの参照に置き換え `NavigationComponent`ます。

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 属性は、 `@Input() items` から、ナビゲーション `HeaderComponent` を構築する `NavigationComponent` 場所に渡します。

6. を開 `header.component.spec.ts` き、次の宣言を追加し `NavigationComponent`ます。

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

   は現在、の一部 `NavigationComponent` として使用されているので、テストベッドの一部として宣言する必要 `HeaderComponent` があります。

7. 開いているファイルに変更を保存し、 **webpack Devサーバに戻ります**。 [http://localhost:4200/](http://localhost:4200/)

   ![完了したヘッダーナビゲーション](assets/navigation-routing/completed-header.png)

   メニューの切り替えをクリックしてナビゲーションを開くと、表示されたナビゲーションリンクが表示されます。 別の表示のSPAに移動できる必要があります。

## SPAルーティングの理解

ナビゲーションが実装されたら、AEMでルーティングを検査します。

1. IDEで、次の場所にあるファイル `app-routing.module.ts` を開き `ui.frontend/src/app`ます。

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

   この `routes: Routes = [];` 配列は、Angularコンポーネントマッピングへのルートまたはナビゲーションパスを定義します。

   `AemPageMatcher` は、このAngularアプリケーションの一部であるAEMのページに「見える」ものに一致する、カスタムのAngularルータ [UrlMatcher](https://angular.io/api/router/UrlMatcher)です。

   `PageComponent` は、AEMのページを表すAngularコンポーネントで、一致したルートが呼び出されます。 さら `PageComponent` に検査を受ける

   `AemPageDataResolver`AEM SPA Editor JS SDKで提供される、カスタムの [Angular Router Resolver](https://angular.io/api/router/Resolve) 。このリゾルバーは、拡張子よりもページパスが小さいAEMのリソースパスに、AEM内のパスであるルートURLを変換するために使用されます。

   例えば、は、ルートのURLをのパス `AemPageDataResolver` に `content/wknd-spa-angular/us/en/home.html` 変換し `/content/wknd-spa-angular/us/en/home`ます。 これは、JSONモデルAPI内のパスに基づいてページのコンテンツを解決するために使用されます。

   `AemPageRouteReuseStrategy`AEM SPA Editor JS SDKで提供される、カスタムのRouteReuseStrategy [。ルート](https://angular.io/api/router/RouteReuseStrategy)`PageComponent` 間での再利用を防ぎます。 そうしないと、ページ「B」に移動する際に、ページ「A」のコンテンツが表示されることがあります。

2. Open the file `page.component.ts` at `ui.frontend/src/app/components/page/`.

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

   は、AEMから取得したJSONを処理するために必要 `PageComponent` で、ルートをレンダリングするAngularコンポーネントとして使用されます。

   `ActivatedRoute`は、Angular Routerモジュールによって提供され、AEMページのJSONコンテンツをどのページのJSONコンテンツをこのAngular Pageコンポーネントインスタンスにロードするかを示す状態を含みます。

   `ModelManagerService`では、ルートに基づいてJSONデータを取得し、データをクラス変数 `path`、 `items`、にマッピングし `itemsOrder`ます。 これらは、 [AEMPageComponentに渡されます](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Open the file `page.component.html` at `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` には、 [AEMPageComponentが含まれます](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)。 変数 `path`、、 `items`および `itemsOrder` がに渡され `AEMPageComponent`ます。 は、SPAエディタJavaScript SDKから提供 `AemPageComponent`され、このデータを反復し、「コンポーネントの [マップ](./map-components.md)」チュートリアルに示すように、JSONデータに基づいてAngularコンポーネントを動的にインスタンス化します。

   は、実際 `PageComponent` にはAngularコンポーネントのプロキシに過ぎま `AEMPageComponent``AEMPageComponent` せん。これは、重いリフティングの大部分で、JSONモデルをAngularコンポーネントに正しくマッピングするためのものです。

## AEMのSPAルーティングInspect

1. ターミナルを開き、 **webpack devサーバーが起動している場合は停止し** ます。 プロジェクトのルートに移動し、Mavenスキルを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Angularプロジェクトでは、非常に厳密なリント規則が有効になっています。 Mavenのビルドに失敗した場合は、エラーを確認し、一覧に表示されているファイルで **Lintエラーが見つからないかを確認します。**」を選択します。リンターで見つかった問題を修正し、Mavenコマンドを再実行します。

2. AEMのSPAホームページに移動します。 [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) 、ブラウザーの開発者ツールを開きます。 以下のスクリーンショットは、Google Chromeブラウザーからキャプチャされたものです。

   ページを更新すると、SPAルート `/content/wknd-spa-angular/us/en.model.json`であるXHRリクエストが表示されます。 チュートリアルで前述したSPAルートテンプレートに対する階層の深さの設定に基づいて、3つの子ページのみが含まれることに注意してください。 これには **ページ3は含まれません**。

   ![初期JSONリクエスト — SPAルート](assets/navigation-routing/initial-json-request.png)

3. 開発ツールを開き、 **3ページに移動します**。

   ![ページ3のナビゲート](assets/navigation-routing/page-three-navigation.png)

   新しいXHRリクエストが行われた対象は次のとおりです。 `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![ページ3 XHRリクエスト](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Managerは、 **ページ3** JSONコンテンツが使用できないことを認識し、追加のXHRリクエストを自動的にトリガーします。

4. さまざまなナビゲーション・リンクを使用して、SPA内を移動し続けます。 追加のXHRリクエストが行われないこと、および完全なページ更新が行われないことを確認します。 これにより、SPAがエンドユーザーに対して高速になり、AEMに返される不要な要求が減ります。

   ![ナビゲーションの実装](assets/navigation-routing/final-navigation-implemented.gif)

5. 次の場所に直接移動して、ディープリンクをテストします。 [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). ブラウザーの「戻る」ボタンが引き続き機能することを確認します。

## バリデーターが{#congratulations}

SPAエディタSDKを使用してAEMページにマッピングすることで、SPAで複数の表示をサポートする方法を学びました。 動的なナビゲーションは、Angularルーティングを使用して実装され、コンポー `Header` ネントに追加されました。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/navigation-routing-solution`ます。

### 次の手順 {#next-steps}

[カスタムコンポーネントの作成](custom-component.md) - AEM SPAエディタで使用するカスタムコンポーネントの作成方法を学びます。 JSONモデルを拡張してカスタムコンポーネントに入力するための作成者ダイアログとSlingモデルの開発方法を学びます。
