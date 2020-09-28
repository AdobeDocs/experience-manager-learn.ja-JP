---
title: SPAコンポーネントのAEMコンポーネントへのマッピング | AEM SPAエディターとAngularの使用の手引き
description: AEM SPA Editor JS SDKを使用して、角度コンポーネントをAdobe Experience Manager(AEM)コンポーネントにマップする方法を学びます。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。
sub-product: サイト
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 1%

---


# SPAコンポーネントのAEMコンポーネントへのマッピング {#map-components}

AEM SPA Editor JS SDKを使用して、角度コンポーネントをAdobe Experience Manager(AEM)コンポーネントにマップする方法を学びます。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。

この章では、AEM JSONモデルAPIについて詳しく説明し、AEMコンポーネントによって公開されたJSONコンテンツを、propとしてAngularコンポーネントに自動的に挿入する方法について説明します。

## 目的

1. AEMコンポーネントをSPAコンポーネントにマップする方法を説明します。
2. **コンテナコンポーネントと** コンテンツ **** コンポーネントの違いを理解します。
3. 既存のAEMコンポーネントにマップする新しいAngularコンポーネントを作成します。

## 作成する内容

この章では、提供された `Text` SPAコンポーネントがAEMコンポー `Text`ネントにどのようにマッピングされるかを調べます。 新しい `Image` SPAコンポーネントが作成され、SPAで使用してAEMで作成できます。 レイア **ウトコンテナ** および **テンプレートエディター** ポリシーの初期設定の機能は、外観が少し異なる表示の作成にも使用されます。

![チャプターサンプル最終オーサリング](./assets/map-components/final-page.png)

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/map-components-solution`ます。

## マッピング手法

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマップすることです。 AEMコンポーネントでは、サーバーサイドで実行し、JSONモデルAPIの一部としてコンテンツを書き出します。 JSONコンテンツは、ブラウザーでクライアント側を実行するSPAで使用されます。 SPAコンポーネントとAEMコンポーネント間の1:1マッピングが作成されます。

![AEMコンポーネントをAngularコンポーネントにマッピングする際の概要](./assets/map-components/high-level-approach.png)

*AEMコンポーネントをAngularコンポーネントにマッピングする際の概要*

## テキストコンポーネントのInspect

AEM [プロジェクトのアーキタイプ](https://github.com/adobe/aem-project-archetype) には `Text` 、AEM [Textコンポーネントにマッピングされるコンポーネントが用意されています](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/text.html)。 これは、AEMの **コンテンツをレンダリングする** content *コンポーネントの例です* 。

コンポーネントの動作を見てみましょう。

### JSONモデルのInspect

1. SPAコードにジャンプする前に、AEMが提供するJSONモデルを理解することが重要です。 コ [アコンポーネントライブラリに移動し](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) 、テキストコンポーネントのページを表示します。 コアコンポーネントライブラリは、すべてのAEMコアコンポーネントの例を提供します。
2. 次のいずれかの例の「 **JSON** 」タブを選択します。

   ![テキストJSONモデル](./assets/map-components/text-json.png)

   次の3つのプロパティが表示されます。 `text`、 `richText`および `:type`。

   `:type` は、AEMコンポーネントの `sling:resourceType` （またはパス）をリストする予約済みのプロパティです。 の値 `:type` は、AEMコンポーネントをSPAコンポーネントにマップするために使用される値です。

   `text` および `richText` は、SPAコンポーネントに公開される追加のプロパティです。

### テキストコンポーネントのInspect化

1. 新しいターミナルを開き、プロジェクト内の `ui.frontend` フォルダに移動します。 WebPack Devサーバ `npm install` を実行 `npm start` し、 **開始に移動します**。

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   モジュ `ui.frontend` ールは、現在、 [モックJSONモデルを使用するように設定されています](./integrate-spa.md#mock-json)。

2. 新しいブラウザウィンドウが開いてhttp://localhost:4200/content/wknd-spa-angular/us/en/home.htmlが表示され [ます。](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![モックコンテンツを含むWebpackデベロッパーサーバー](assets/map-components/initial-start.png)

3. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 モジュールを展開し、次の下にあるファイル `ui.frontend` text.component.ts ****`ui.frontend/src/app/components/text/text.component.ts`を開きます。

   ![Text.js Angularコンポーネントのソースコード](assets/map-components/vscode-ide-text-js.png)

4. 最初に調査する領域は、～ `class TextComponent` 行35:

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

   [@Input()](https://angular.io/api/core/Input) decoratorは、マップされたJSONオブジェクトを介して値が設定されるフィールドを宣言するために使用します。

   `@HostBinding('innerHtml') get content()` は、の値からオーサリング済みのテキストコンテンツを公開するメソッド `this.text`です。 コンテンツがリッチテキストの場合( `this.richText` フラグによって決まる)、Angularの組み込みセキュリティは迂回されます。 Angularの [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) は、生のHTMLを「スクラブ」し、クロスサイトスクリプティングの脆弱性を防ぐために使用されます。 このメソッドは、 `innerHtml` @HostBinding [](https://angular.io/api/core/HostBinding) デコレーターを使用してプロパティにバインドされます。

5. 次に、～ `TextEditConfig` 行24の

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上記のコードは、AEM作成者環境でプレースホルダーをレンダリングするタイミングを決定する役割を持ちます。 この `isEmpty` メソッドが **trueを返す場合** 、プレースホルダーがレンダリングされます。

6. 最後に、～line 53の `MapTo` コールを見てみましょう。

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** は、AEM SPA Editor JS SDK(`@adobe/cq-angular-editable-components`)で提供されます。 パスはAEMコンポーネント `wknd-spa-angular/components/text``sling:resourceType` のパスを表します。 このパスは、前に確認したJSONモデルに `:type` よって公開されたパスと一致します。 **MapToは** 、JSONモデルの応答を解析し、正しい値をSPAコンポーネントの `@Input()` 変数に渡します。

   AEM `Text` コンポーネントの定義は、で確認でき `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`ます。

7. で **en.model.json** ファイルを変更してテストし `ui.frontend/src/mocks/json/en.model.json`ます。

   ～line 62で、最初の `Text` 値を更新してタグ **`H1`** と **`u`** タグを使用します。

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   ブラウザに戻り、 **webpack devサーバが提供する効果を確認します**。

   ![更新されたテキストモデル](assets/map-components/updated-text-model.png)

   プロパティを `richText` true **/****** falseの間で切り替えて、実行中のレンダリングロジックを確認してください。

8. Inspect **text.component.html** ( `ui.frontend/src/app/components/text/text.component.html`ここ)

   コンポーネントの内容全体がプロパティによって設定されるので、このファイルは空で `innerHTML` す。

9. app. **module.tsのInspect**`ui.frontend/src/app/app.module.ts`。

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

   TextComponent **は明示的に含まれるのではなく、AEM SPA Editor JS SDKが提供するAEMResponsiveGridComponent** ( **AEMResponsiveGridComponent** )を介して動的に含められます。 したがって、 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 配列に指定する必要があります。

## 画像コンポーネントの作成

次に、AEM `Image` Imageコンポーネントにマップされる [Angularコンポーネントを作成します](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html)。 この `Image` コンポーネントは、 **content** componentの別の例です。

### InspectJSON

SPAコードにジャンプする前に、AEMが提供するJSONモデルを調べます。

1. コアコンポーネントライブラリの [画像の例に移動します](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)。

   ![Image CoreコンポーネントJSON](./assets/map-components/image-json.png)

   SPAコンポー `src`ネントの設定に、、、のプロパティ `alt``title``Image` が使用されます。

   >[!NOTE]
   >
   > 開発者がアダプティブコンポーネントと遅延読み込みコンポーネントを作成できるように、公開されているその他の画像プロパティ(`lazyEnabled`、 `widths`)があります。 このチュートリアルで作成するコンポーネントは簡単で、これらの高度なプロパティは **使用しません** 。

2. IDEに戻り、atを開き `en.model.json` ま `ui.frontend/src/mocks/json/en.model.json`す。 これはプロジェクトの新しいコンポーネントなので、画像JSONを「モック」する必要があります。

   ～line 70で、 `image` モデルにJSONエントリを追加し(2番目のカンマの後のカンマを忘れないでください `,` )、 `text_386303036``:itemsOrder` 配列を更新します。

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

   このプロジェクトには、 `/mock-content/adobestock-140634652.jpeg` webpack開発サーバで使用 **するサンプルイメージが含まれています**。

   完全な [en.model.jsonをここで表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json)。

3. コンポ追加ーネントが表示するストック写真。

   下に **images** という名前の新しいフォルダを作成 `ui.frontend/src/mocks`します。 adobestock-140634652.jpeg [をダウンロードし](assets/map-components/adobestock-140634652.jpeg) 、新しく作成した **画像** フォルダーに配置します。 必要に応じて、自由に自分の画像を使用できます。

### 画像コンポーネントの実装

1. 起動している場合は、 **webpack devサーバーを停止し** ます。
2. 新しいイメージコンポーネントを作成するには、次のフォルダ内からAngular CLI `ng generate component` コマンドを実行し `ui.frontend` ます。

   ```shell
   $ ng generate component components/image
   ```

3. IDEで、 **image.component.tsを開き** 、次のように更新 `ui.frontend/src/app/components/image/image.component.ts` します。

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

   `ImageEditConfig` は、プ `src` ロパティが設定されているかどうかに基づいて、AEMで作成者プレースホルダーをレンダリングするかどうかを決定する設定です。

   `@Input()` の、、 `src`およびはJSON APIからマッピング `alt``title` されたプロパティです。

   `hasImage()` は、イメージをレンダリングする必要があるかどうかを決定するメソッドです。

   `MapTo` SPAコンポーネントをにあるAEMコンポーネントにマップし `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`ます。

4. image.component.html **を開き** 、次のように更新します。

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   これは、 `<img>` trueが返された場合に `hasImage` 要素をレンダリングし **ます**。

5. image.component.scss **を開き** 、次のように更新します。

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
   > この `:host-context` ルールは、AEM SPAエディタのプレースホルダが正しく機能するために **** 重要です。 AEMページエディターで作成するSPAコンポーネントはすべて、少なくともこのルールが必要です。

6. を開 `app.module.ts` き、 `ImageComponent` アレイ `entryComponents` に追加します。

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   と同様 `TextComponent`に、は動的にロード `ImageComponent` され、 `entryComponents` 配列に含める必要があります。

7. レンダリングを確認する **webpack devサーバ** を開始 `ImageComponent` します。

   ```shell
   $ npm run start:mock
   ```

   ![画像が](assets/map-components/image-added-mock.png)

   *SPAに追加されたイメージ*

   >[!NOTE]
   >
   > **ボーナスの課題**:の値を画像の下にキャプションとして表示する新しいメソッド `title` を実装しました。

## AEMでのポリシーの更新

コンポー `ImageComponent` ネントは、 **webpack開発サーバでのみ表示されます**。 次に、更新したSPAをAEMにデプロイし、テンプレートポリシーを更新します。

1. Webpackデベロッパーサーバーを停止し **、プロジェクトの** ルートから **** 、Mavenスキルを使用してAEMに変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM開始画面で、 **[!UICONTROL ツール]** / **[!UICONTROL テンプレート]** / **[WKND SPA Angularに移動します](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**。

   「 **SPA Page**」を選択して編集します。

   ![SPAページテンプレートの編集](assets/map-components/edit-spa-page-template.png)

3. レイ **アウトコンテナを選択し** 、「 **ポリシー** 」アイコンをクリックして、ポリシーを編集します。

   ![レイアウトコンテナポリシー](./assets/map-components/layout-container-policy.png)

4. 「 **許可されているコンポーネント** 」 **>「** WKND SPA Angular - Content **」の下で、「** Image」コンポーネントを確認します。

   ![画像コンポーネントが選択されました](assets/map-components/check-image-component.png)

   「 **デフォルトコンポーネント** 」( **Default Components** ) > 「 **マッピング」(Mapping** )の下で、「Image - WKND SPA Angular - Content」（イメージ — WKND SPA Angular - Content）コンポーネントを選択します。

   ![デフォルトコンポーネントの設定](assets/map-components/default-components.png)

   の **MIMEタイプを入力し**`image/*`ます。

   「 **完了** 」をクリックして、ポリシーの更新を保存します。

5. レイ **アウトコンテナで** 、「 **Text** 」コンポーネントのポリシー **** アイコンをクリックします。

   ![テキストコンポーネントポリシーアイコン](./assets/map-components/edit-text-policy.png)

   Create a new policy named **WKND SPA Text**. 「 **プラグイン** / **書式設定** /すべてのチェックボックスをオンにして、追加の書式設定オプションを有効にします。

   ![RTE書式を有効にする](assets/map-components/enable-formatting-rte.png)

   「 **プラグイン** / **段落スタイル** 」で、「段落スタイルを **有効にする」チェックボックスをオンにします**。

   ![段落スタイルを有効にする](./assets/map-components/text-policy-enable-paragraphstyles.png)

   「 **完了** 」をクリックして、ポリシーの更新を保存します。

6. ホーム **ページ** http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.htmlに移動します [](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。

   また、 `Text` コンポーネントを編集し、 **フルスクリーンモードで段落スタイルを追加することもできます** 。

   ![フルスクリーンリッチテキストの編集](assets/map-components/full-screen-rte.png)

7. また、 **アセットファインダーから画像をドラッグ&amp;ドロップできる必要があります**。

   ![画像のドラッグ&amp;ドロップ](./assets/map-components/drag-drop-image.gif)

8. [AEM Assetsを追加通じて独自の画像を作成するか](http://localhost:4502/assets.html/content/dam) 、標準の [WKNDリファレンスサイト用の完成したコードベースをインストールします](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKNDリファレンスサイト [には](https://github.com/adobe/aem-guides-wknd/releases/latest) 、WKND SPAで再利用できる多くの画像が含まれています。 パッケージは、 [AEM Package Managerを使用してインストールできます](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

## レイアウトコンテナのInspect

レイ **アウトコンテナのサポートは** 、AEM SPA Editor SDKによって自動的に提供されます。 名前で示される **レイアウトコンテナ**&#x200B;は **、** コンテナコンポーネントです。 コンテナコンポーネントは、 *他のコンポーネントを表すJSON構造を受け入れ、動的にインスタンス化するコンポーネントで* す。

レイアウトコンテナをさらに詳しく見てみましょう。

1. IDEで、 **responsive-grid.component.tsを開きます** ( `ui.frontend/src/app/components/responsive-grid`次の場所)。

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   は、AEM SPA Editor SDKの一部として実装 `AEMResponsiveGridComponent` され、を介してプロジェクトに含まれ `import-components`ます。

2. ブラウザーで、http://localhost:4502/content/wknd-spa-angular/us/en.model.jsonに移動し [ます。](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSONモデルAPI — レスポンシブグリッド](./assets/map-components/responsive-grid-modeljson.png)

   レイ **アウトコンテナ** コンポーネントは、のを持ち、SPAエディタでは、のコンポーネント `sling:resourceType` と同様に、 `wcm/foundation/components/responsivegrid` プロパティを使用して認識され `:type``Text``Image` ます。

   レイ [アウトモードを使用したコンポーネントのサイズ変更と同じ機能をSPAエディタで使用できます](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 。

3. http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.htmlに戻り [ます](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 その他の **画像** コンポーネントを追加追加し、「 **レイアウト** 」オプションを使用してサイズを変更してみてください。

   ![レイアウトモードを使用した画像のサイズ変更](./assets/map-components/responsive-grid-layout-change.gif)

4. JSONモデルhttp://localhost:4502/content/wknd-spa-angular/us/en.model.jsonを再度開き [、JSONの一部](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)`columnClassNames` としての値を確認します。

   ![列クラス名](./assets/map-components/responsive-grid-classnames.png)

   クラス名は、12列のグリッドに基づいて4列幅のコンポーネントである必要があることを `aem-GridColumn--default--4` 示します。 レス [ポンシブグリッドについて詳しくは、こちらを参照してください](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

5. IDEに戻り、 `ui.apps` モジュール内に、に定義されたクライアント側ライブラリがあり `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`ます。 Open the file `less/grid.less`.

   このファイルは、`default`レイアウトコンテナで使用されるブレークポイント( `tablet`、 `phone`および **)を決定します**。 このファイルは、プロジェクト仕様に合わせてカスタマイズすることを目的としています。 現在、ブレークポイントはと `1200px` に設定されて `650px`います。

6. コンポーネントのレスポンシブ機能と更新されたリッチテキストポリシーを使用して、次のような表示を作成できる `Text` はずです。

   ![チャプターサンプル最終オーサリング](assets/map-components/final-page.png)

## バリデーターが{#congratulations}

これで、SPAコンポーネントをAEMコンポーネントにマップする方法を学習し、新しい `Image` コンポーネントを実装しました。 また、 **レイアウトコンテナのレスポンシブ機能を調べる機会もありました**。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/map-components-solution`ます。

### 次の手順 {#next-steps}

[ナビゲーションとルーティング](navigation-routing.md) - SPAエディターSDKを使用してAEMページにマッピングすることで、SPA内の複数の表示をサポートする方法を学びます。 ダイナミックナビゲーションはAngular Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。

## ボーナス：構成をソース管理に永続的に {#bonus}

多くの場合、特にAEMプロジェクトの最初に、テンプレートや関連するコンテンツポリシーなどの設定を保持してソース管理を行うことが重要です。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性を確保できます。 プロジェクトが一定の成熟度に達したら、テンプレートの管理方法をパワーユーザーの特別なグループに変えることができます。

次のいくつかの手順は、Visual Studio Code IDEと [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) を使用して行われますが **、AEMのローカルインスタンスからコンテンツを** 取り込んだり **** 読み込んだりするように設定した任意のツールとIDEを使用して行うことができます。

1. Visual StudioコードIDEで、Marketplace拡張機能を使用して **VSCode AEM Sync** がインストールされていることを確認します。

   ![VSCode AEM同期](./assets/map-components/vscode-aem-sync.png)

2. プロジェクトエクスプローラで **ui.content** モジュールを展開し、に移動し `/conf/wknd-spa-angular/settings/wcm/templates`ます。

3. **フォルダーを右クリックし** 、 `templates` 「AEM Server ****:

   ![VSCodeインポートテンプレート](assets/map-components/import-aem-servervscode.png)

4. コンテンツを読み込む手順を繰り返しますが、にある **ポリシー** フォルダーを選択し `/conf/wknd-spa-angular/settings/wcm/templates/policies`ます。

5. フ `filter.xml` ァイルをInspectに配置し `ui.content/src/main/content/META-INF/vault/filter.xml`ます。

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

   この `filter.xml` ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルター `mode="merge"` に、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示すが、 コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントでコンテンツが上書きされないこ **とが重要です** 。 フィルタ要素の操作の詳細については、 [FileVaultのドキュメント](https://jackrabbit.apache.org/filevault/filter.html) を参照してください。

   各モジュール `ui.content/src/main/content/META-INF/vault/filter.xml` で管理され `ui.apps/src/main/content/META-INF/vault/filter.xml` る各ノードを比較し、理解します。
