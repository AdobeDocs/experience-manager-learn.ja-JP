---
title: SPAコンポーネントのAEMコンポーネントへのマッピング | AEM SPA Editor とAngularの概要
description: AEM SPA Editor JS SDK を使用して、AngularコンポーネントをAdobe Experience Manager(AEM) コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、AEM SPA エディター内で、従来の AEM オーサリングと同様に、SPA コンポーネントを動的に更新できます。
feature: SPA Editor
version: Cloud Service
jira: KT-5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
duration: 711
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2213'
ht-degree: 54%

---

# SPA コンポーネントを AEM コンポーネントへのマッピング {#map-components}

AEM SPA Editor JS SDK を使用して、AngularコンポーネントをAdobe Experience Manager(AEM) コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、AEM SPA エディター内で、従来の AEM オーサリングと同様に、SPA コンポーネントを動的に更新できます。

この章では、AEM JSON モデル API について詳しく説明し、AEMコンポーネントによって公開された JSON コンテンツを prop としてAngularコンポーネントに自動的に挿入する方法について説明します。

## 目的

1. AEM コンポーネントを SPA コンポーネントにマッピングする方法について説明します。
2. 違いを理解する **コンテナ** コンポーネントと **コンテンツ** コンポーネント。
3. 既存のAngularコンポーネントにマッピングする新しいAEMコンポーネントを作成します。

## 作成する内容

この章では、指定された `Text` SPAコンポーネントがAEMにマッピングされている `Text`コンポーネント。 新しい `Image` SPAコンポーネントが作成され、SPAで使用してAEMでオーサリングできます。 標準搭載のの機能 **レイアウトコンテナ** および **テンプレートエディター** ポリシーを使用して、外観が少し多様なビューを作成することもできます。

![この章の最終的なオーサリングサンプル](./assets/map-components/final-page.png)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

### コードの取得

1. このチュートリアルの出発点となるものを Git からダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Maven を使用してコードベースをローカルの AEM インスタンスにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility) を使用する場合、次の `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

完成したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) で確認するか、またはブランチ `Angular/map-components-solution` に切り替えてコードをローカルで確認します。

## マッピングアプローチ

基本的な概念は、SPA コンポーネントを AEM コンポーネントにマッピングすることです。AEM コンポーネントは、サーバーサイドで実行され、JSON モデル API の一部としてコンテンツを書き出します。JSON コンテンツは、ブラウザーでクライアントサイドを実行している SPA によって使用されます。SPA コンポーネントと AEM コンポーネントの間に 1 対 1 のマッピングが作成されます。

![AEMコンポーネントとAngularコンポーネントのマッピングの概要](./assets/map-components/high-level-approach.png)

*AEMコンポーネントとAngularコンポーネントのマッピングの概要*

## テキストコンポーネントを検査する

[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)は、AEM [テキストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=ja)にマッピングされる `Text` コンポーネントを提供します。これは、AEM から&#x200B;*コンテンツ*&#x200B;をレンダリングするという、**コンテンツ**&#x200B;コンポーネントの例です。

コンポーネントの動作を見てみましょう。

### JSON モデルを調べる

1. SPA のコードを調べる前に、AEM が提供する JSON モデルを理解しておくことが重要です。 [コアコンポーネントライブラリ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html)に移動し、テキストコンポーネントのページを表示します。コアコンポーネントライブラリには、すべての AEM コアコンポーネントの例が記載されています。
2. 例の 1 つである **JSON** タブを選択します。

   ![テキスト JSON モデル](./assets/map-components/text-json.png)

   `text`、`richText`、および `:type` の 3 つのプロパティが表示されます。

   `:type` は、AEM コンポーネントの `sling:resourceType`（またはパス）をリストする予約済みのプロパティです。`:type` の値は、AEM コンポーネントを SPA コンポーネントにマップするために使用されるものです。

   `text` および `richText` は、SPA コンポーネントに公開される追加のプロパティです。

### Inspect Text コンポーネント

1. 新しいターミナルを開き、 `ui.frontend` フォルダーをプロジェクト内に配置します。 実行 `npm install` その後 `npm start` を起動します。 **webpack dev server**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   The `ui.frontend` モジュールは現在、 [モック JSON モデル](./integrate-spa.md#mock-json).

2. 新しいブラウザーウィンドウが開いて、 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![モックコンテンツを含む Webpack Dev サーバー](assets/map-components/initial-start.png)

3. 任意の IDE で、WKND SPA 用の AEM プロジェクトを開きます。 を展開します。 `ui.frontend` モジュールを開き、ファイルを開きます。 **text.component.ts** under `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.jsAngularコンポーネントのソースコード](assets/map-components/vscode-ide-text-js.png)

4. 最初に検査する領域は、 `class TextComponent` ～行 35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) decorator は、既に確認した、マッピングされた JSON オブジェクトを介して値が設定されるフィールドを宣言するために使用されます。

   `@HostBinding('innerHtml') get content()` は、作成したテキストコンテンツを `this.text`. コンテンツがリッチテキストの場合 ( `this.richText` フラグ )Angularの組み込みのセキュリティは無視されます。 Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) は、生のHTMLを「スクラブ」し、クロスサイトスクリプティングの脆弱性を防ぐために使用されます。 メソッドは、 `innerHtml` プロパティを [@HostBinding](https://angular.io/api/core/HostBinding) デコレーター。

5. 次に、 `TextEditConfig` ～行 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上記のコードは AEM オーサー環境で、プレースホルダーをレンダリングするタイミングを決定する役割を果たします。 この `isEmpty` メソッドが **true** を返した場合、プレースホルダーがレンダリングされます。

6. 最後に、53 行目までの `MapTo` 呼び出しを確認します。

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** は、AEM SPA Editor JS SDK(`@adobe/cq-angular-editable-components`) をクリックします。 パス `wknd-spa-angular/components/text` は、AEM コンポーネントの `sling:resourceType` を表します。このパスは、前に確認した JSON モデルによって公開された `:type` と一致します。**MapTo** は JSON モデルの応答を解析し、正しい値を `@Input()` SPAコンポーネントの変数。

   AEM `Text` コンポーネントの定義は `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text` にあります。

7. を変更して実験する **en.model.json** ～にファイルを送る `ui.frontend/src/mocks/json/en.model.json`.

   ～ライン 62 で、最初の `Text` 使用する値 **`H1`** および **`u`** タグ：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   ブラウザーに戻り、 **webpack dev server**:

   ![更新されたテキストモデル](assets/map-components/updated-text-model.png)

   以下を切り替えてみてください： `richText` 間のプロパティ **true** / **false** をクリックして、実行中のレンダリングロジックを確認します。

8. Inspect **text.component.html** 時刻 `ui.frontend/src/app/components/text/text.component.html`.

   コンポーネントのコンテンツ全体が `innerHTML` プロパティ。

9. Inspect **app.module.ts** 時刻 `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   The **TextComponent** が明示的に含まれていないが、より動的に **AEMResponsiveGridComponent** は、AEM SPA Editor JS SDK によって提供されます。 したがって、 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 配列。

## 画像コンポーネントの作成

次に、 `Image` AEMにマッピングされるangularコンポーネント [画像コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=ja). この `Image` コンポーネントは、**コンテンツ**&#x200B;コンポーネントのもう一つの例です。

### JSO の検査

SPA コードを調べる前に、AEM が指定した JSON モデルを調べます。

1. [コアコンポーネントライブラリに置かれた画像の例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html)に移動します。

   ![画像コアコンポーネントの JSON](./assets/map-components/image-json.png)

   `src`、`alt`、`title` のプロパティは、SPA `Image` コンポーネントの入力に使用されます。

   >[!NOTE]
   >
   > 公開された他の画像プロパティ（`lazyEnabled`、`widths`）があり、これを使用して開発者は、適応型の遅延読み込みコンポーネントを作成することができます。このチュートリアルで作成されたコンポーネントはシンプルで、これらの詳細なプロパティを使用&#x200B;**しません**。

2. IDE に戻り、 `en.model.json` 時刻 `ui.frontend/src/mocks/json/en.model.json`. これはプロジェクトの新しいコンポーネントなので、画像 JSON のモックを作成する必要があります。

   ～行 70 で、 `image` モデル（末尾のコンマを忘れないでください） `,` 二回目以降 `text_386303036`) をクリックし、 `:itemsOrder` 配列。

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   このプロジェクトには、以下の場所にサンプル画像が含まれています。 `/mock-content/adobestock-140634652.jpeg` と一緒に使用される **webpack dev server**.

   完全な [en.model.json はここに](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. コンポーネントに表示するストック写真を追加します。

   という名前の新しいフォルダーを作成します。 **画像** の下 `ui.frontend/src/mocks`. ダウンロード [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) 新しく作成した **画像** フォルダー。 必要に応じて、自分の画像を自由に使用できます。

### 画像コンポーネントの実装

1. を停止します。 **webpack dev server** （開始した場合）
2. angularCLI を実行して新しい画像コンポーネントを作成する `ng generate component` 内から命令する `ui.frontend` フォルダー：

   ```shell
   $ ng generate component components/image
   ```

3. IDE で、を開きます。 **image.component.ts** 時刻 `ui.frontend/src/app/components/image/image.component.ts` およびを次のように更新します。

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` は、AEMでオーサープレースホルダーをレンダリングするかどうかを、 `src` プロパティが設定されている。

   `@Input()` / `src`, `alt`、および `title` は、JSON API からマッピングされるプロパティです。

   `hasImage()` は、イメージをレンダリングする必要があるかどうかを指定するメソッドです。

   `MapTo` はSPAコンポーネントを、次の場所にあるAEMコンポーネントにマッピングします。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. 開く **image.component.html** を更新し、次のようにします。

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   これにより、 `<img>` 要素の場合 `hasImage` 戻り値 **true**.

5. 開く **image.component.scss** を更新し、次のようにします。

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > The `:host-context` ルール： **重要な** AEM SPAエディターのプレースホルダーが正しく機能するように設定する必要があります。 AEMページエディターで作成するSPAコンポーネントは、すべて、少なくともこのルールが必要です。

6. 開く `app.module.ts` をクリックし、 `ImageComponent` から `entryComponents` 配列：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   次に類似 `TextComponent`、 `ImageComponent` は動的に読み込まれ、 `entryComponents` 配列。

7. を開始します。 **webpack dev server** 見る `ImageComponent` レンダリング。

   ```shell
   $ npm run start:mock
   ```

   ![モックに追加された画像](assets/map-components/image-added-mock.png)

   *画像がSPAに追加されました*

   >[!NOTE]
   >
   > **ボーナスチャレンジ**：の値を表示する新しいメソッドを実装します。 `title` 画像の下のキャプションとして。

## AEMでのポリシーの更新

The `ImageComponent` コンポーネントは、 **webpack dev server**. 次に、更新されたSPAをAEMにデプロイし、テンプレートポリシーを更新します。

1. を停止します。 **webpack dev server** そして **root** を使用して、AEMに変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM Start 画面で、に移動します。 **[!UICONTROL ツール]** > **[!UICONTROL テンプレート]** > **[WKNDSPAAngular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   を選択して編集します。 **SPA Page**:

   ![ページテンプレートをSPA](assets/map-components/edit-spa-page-template.png)

3. **レイアウトコンテナ**&#x200B;を選択し、「**ポリシー**」アイコンをクリックしてポリシーを編集します。

   ![レイアウトコンテナポリシー](./assets/map-components/layout-container-policy.png)

4. の下 **許可されたコンポーネント** > **WKND SPAAngular — コンテンツ** > チェックボックスをオンにして **画像** コンポーネント：

   ![画像コンポーネントが選択されました](assets/map-components/check-image-component.png)

   の下 **デフォルトのコンポーネント** > **マッピングを追加** を選択し、 **画像 — WKND SPAAngular — コンテンツ** コンポーネント：

   ![デフォルトコンポーネントを設定](assets/map-components/default-components.png)

   `image/*` の **mime タイプ**&#x200B;を入力します。

   「**完了**」をクリックして、ポリシーの更新を保存します。

5. Adobe Analytics の **レイアウトコンテナ** をクリックします。 **ポリシー** アイコン **テキスト** コンポーネント：

   ![テキストコンポーネントポリシーアイコン](./assets/map-components/edit-text-policy.png)

   **WKND SPA テキスト**&#x200B;という名前の新しいポリシーを作成します。**プラグイン**／**書式設定**&#x200B;の下にあるすべてのボックスをオンにして、次の追加の書式設定オプションを有効にします。

   ![RTE フォーマットを有効にする](assets/map-components/enable-formatting-rte.png)

   **プラグイン**／**段落スタイル**&#x200B;の下で、「**段落スタイルを有効にする**」チェックボックスをオンにします。

   ![段落スタイルを有効にする](./assets/map-components/text-policy-enable-paragraphstyles.png)

   「**完了**」をクリックしてポリシーの更新を保存します。

6. 次に移動： **ホームページ** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   また、`Text` コンポーネントを編集して、**全画面表示**&#x200B;モードで追加の段落スタイルを追加できます。

   ![全画面表示のリッチテキスト編集](assets/map-components/full-screen-rte.png)

7. また、画像を&#x200B;**アセットファインダー**&#x200B;からドラッグ＆ドロップできます。

   ![画像をドラッグ＆ドロップする](./assets/map-components/drag-drop-image.gif)

8. [AEM Assets](http://localhost:4502/assets.html/content/dam) を介して独自の画像を追加するか、標準の [WKND 参照サイト](https://github.com/adobe/aem-guides-wknd/releases/latest)の完成したコードベースをインストールします。[WKND 参照サイト](https://github.com/adobe/aem-guides-wknd/releases/latest)には、WKND SPAで再利用できる画像が多数含まれています。 パッケージは、[AEM のパッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![パッケージマネージャーによる wknd.all のインストール](./assets/map-components/package-manager-wknd-all.png)

## レイアウトコンテナを調べる

**レイアウトコンテナ**&#x200B;のサポートは、AEM SPA Editor SDK によって自動的に提供されます。 **レイアウトコンテナ**&#x200B;は、その名前が示すように&#x200B;**コンテナ**&#x200B;コンポーネントです。コンテナコンポーネントは、*他の*&#x200B;コンポーネントを表す JSON 構造を受け入れ、それらを動的にインスタンス化するコンポーネントです。

ここでは、レイアウトコンテナをさらに詳しく調べます。

1. IDE でを開きます。 **responsive-grid.component.ts** 時刻 `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   The `AEMResponsiveGridComponent` はAEM SPA Editor SDK の一部として実装され、を介してプロジェクトに含まれます。 `import-components`.

2. ブラウザーで、に移動します。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON モデル API - レスポンシブグリッド](./assets/map-components/responsive-grid-modeljson.png)

   **レイアウトコンテナ**&#x200B;コンポーネントの `sling:resourceType` は `wcm/foundation/components/responsivegrid` であり、`Text` コンポーネントや `Image` コンポーネントと同様に、`:type` プロパティを使用して SPA エディターによって認識されます。

   [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=ja#defining-layouts-layout-mode)を使用してコンポーネントのサイズを変更するのと同じ機能が、SPA エディターで利用できます。

3. 戻る [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). さらに&#x200B;**画像**&#x200B;コンポーネントを追加し、**レイアウト**&#x200B;オプションを使用してサイズを変更してみてください。

   ![レイアウトモードを使用した画像のサイズ変更](./assets/map-components/responsive-grid-layout-change.gif)

4. JSON モデルを再度開く [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) そして見てみろ `columnClassNames` を JSON の一部として使用する場合：

   ![列クラス名](./assets/map-components/responsive-grid-classnames.png)

   クラス名 `aem-GridColumn--default--4` は、12 列のグリッドに基づいて幅が 4 列に設定されている必要があることを示します。レスポンシブグリッドについて詳しくは、[こちら](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)をご覧ください。

5. IDE に戻り、`ui.apps` モジュールで、`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid` で定義されたクライアントサイドライブラリがあります。`less/grid.less` ファイルを開きます。

   このファイルは、**レイアウトコンテナ**&#x200B;で使用されるブレークポイント（`default`、`tablet`、`phone`）を特定します。このファイルは、プロジェクトの仕様に応じてカスタマイズされます。 現在、ブレークポイントは `1200px` および `650px` に設定されています。

6. `Text` コンポーネントのレスポンシブ機能と、更新されたリッチテキストポリシーを使用して、次のようなビューを作成できます。

   ![章サンプルの最終オーサリング](assets/map-components/final-page.png)

## おめでとうございます。 {#congratulations}

これで、SPAコンポーネントをAEMコンポーネントにマッピングする方法を学び、新しい `Image` コンポーネント。 また、**レイアウトコンテナ**&#x200B;のレスポンシブ機能について探索する機会もありました。

完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) で確認する、または、ブランチ `Angular/map-components-solution` に切り替えて、コードをローカルでチェックアウトします。

### 次の手順 {#next-steps}

[ナビゲーションとルーティング](navigation-routing.md) - SPA Editor SDK を使用して AEM ページにマッピングすることで、SPA の複数のビューをサポートする方法について説明します。 動的ナビゲーションは、Angularルーターを使用して実装され、既存のヘッダーコンポーネントに追加されます。

## ボーナス — 構成をソース管理に保持 {#bonus}

多くの場合、特に AEM プロジェクトの開始時に、テンプレートや関連するコンテンツポリシーなどの設定をソースコントロールに保持すると便利です。 これにより、すべての開発者が同じセットのコンテンツと設定に対して作業することが保証され、環境間の一貫性がさらに確保できます。プロジェクトが一定の成熟度に達すると、テンプレートの管理作業を特別なパワーユーザーグループに引き継ぐことができます。

次のいくつかの手順は、Visual Studio Code IDE と [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) を使用して行われますが、AEM のローカルインスタンスからコンテンツを&#x200B;**取り込み**&#x200B;または&#x200B;**読み込む**&#x200B;ように設定した任意のツールと IDE を使用して行うことができます。

1. Visual Studio Code IDE で、Marketplace 拡張機能を介して **VSCode AEM Sync** がインストールされていることを確認します。

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. プロジェクトエクスプローラー内の **ui.content** モジュールをを展開して、`/conf/wknd-spa-angular/settings/wcm/templates` に移動します。

3. `templates` フォルダーを&#x200B;**右クリック**&#x200B;し、**AEM サーバーから読み込み**&#x200B;を選択します。

   ![VSCode 読み込みテンプレート](assets/map-components/import-aem-servervscode.png)

4. コンテンツを読み込む手順を繰り返しますが、`/conf/wknd-spa-angular/settings/wcm/policies` に置かれた&#x200B;**ポリシー**&#x200B;フォルダーを選択します。

5. `ui.content/src/main/content/META-INF/vault/filter.xml` にある `filter.xml` ファイルを調べます。

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   `filter.xml` ファイルは、パッケージと共にインストールされるノードのパスを識別する役割を果たします。各フィルターの `mode="merge"` に注目してください。これは、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示しています。コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントでは、コンテンツを上書き&#x200B;**しない**&#x200B;ことが重要です。フィルター要素の操作の詳細については、[FileVault ドキュメント](https://jackrabbit.apache.org/filevault/filter.html)を参照してください。

   `ui.content/src/main/content/META-INF/vault/filter.xml` と `ui.apps/src/main/content/META-INF/vault/filter.xml` を比較して、各モジュールによって管理される様々なノードを理解します。
