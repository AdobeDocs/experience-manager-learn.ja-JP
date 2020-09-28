---
title: SPAコンポーネントのAEMコンポーネントへのマッピング | AEM SPAエディターとReactの使い始めに
description: AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。
sub-product: サイト
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 2%

---


# SPAコンポーネントのAEMコンポーネントへのマッピング {#map-components}

AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。

この章では、AEM JSONモデルAPIについて詳しく説明し、AEMコンポーネントによって公開されたJSONコンテンツを、propとして自動的にReactコンポーネントに挿入する方法について説明します。

## 目的

1. AEMコンポーネントをSPAコンポーネントにマップする方法を説明します。
2. **コンテナコンポーネントと** コンテンツ **** コンポーネントの違いを理解します。
3. 既存のAEMコンポーネントにマッピングする新しいReactコンポーネントを作成します。

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
   $ git checkout React/map-components-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/map-components-solution`ます。

## マッピング手法

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマップすることです。 AEMコンポーネントでは、サーバーサイドで実行し、JSONモデルAPIの一部としてコンテンツを書き出します。 JSONコンテンツは、ブラウザーでクライアント側を実行するSPAで使用されます。 SPAコンポーネントとAEMコンポーネント間の1:1マッピングが作成されます。

![AEMコンポーネントとReactコンポーネントのマッピングの概要](./assets/map-components/high-level-approach.png)

*AEMコンポーネントとReactコンポーネントのマッピングの概要*

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

1. 新しいターミナルを開き、プロジェクト内の `ui.frontend` フォルダに移動します。 webpack-dev-server `npm install` を実行 `npm start` し、開始に移動します ****。

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   モジュ `ui.frontend` ールは、現在、 [モックJSONモデルを使用するように設定されています](./integrate-spa.md#mock-json)。

2. 新しいブラウザウィンドウが開いてhttp://localhost:3000/content/wknd-spa-react/us/en/home.htmlが表示され [ます。](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![モックコンテンツを含むWebpackデベロッパーサーバー](./assets/map-components/initial-start.png)

3. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 モジュールを展開し、次の `ui.frontend` 下にあるファイル `Text.js` を開き `ui.frontend/src/components/Text/Text.js`ます。

   ![Text.jsリアクションコンポーネントのソースコード](./assets/map-components/vscode-ide-text-js.png)

4. 最初に調査する領域は、～ `class Text` 行40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` は、標準的なReactコンポーネントです。 コンポーネントは、を使用 `this.props.richText` して、レンダリングするコンテンツをリッチテキストにするかプレーンテキストにするかを決定します。 実際に使用される「コンテンツ」は、次の場所から取得され `this.props.text`ます。 潜在的なXSS攻撃を防ぐために、リッチテキストは、危険なSetInnerHTMLを使用してコンテンツをレンダリング `DOMPurify` する [](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 前に、経由でエスケープされます。 前述のJSONモデルから `richText` と `text` プロパティを呼び出します。

5. 次に、～line 29 `TextEditConfig` の

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上記のコードは、AEM作成者環境でプレースホルダーをレンダリングするタイミングを決定する役割を持ちます。 この `isEmpty` メソッドが **trueを返す場合** 、プレースホルダーがレンダリングされます。

6. 最後に、～line 62の `MapTo` コールを見てみましょう。

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` は、AEM SPAエディターJS SDK(`@adobe/aem-react-editable-components`)によって提供されます。 パスはAEMコンポーネント `wknd-spa-react/components/text``sling:resourceType` のパスを表します。 このパスは、前に確認したJSONモデルに `:type` よって公開されたパスと一致します。 `MapTo` は、JSONモデルの応答を解析し、正しい値をSPAコンポーネントとして渡す処理 `props` を行います。

   AEM `Text` コンポーネントの定義は、で確認でき `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`ます。

7. でフ `mock.model.json` ァイルを変更してテストし `ui.frontend/public/mock-content/mock.model.json`ます。 ～line 62で、最初の `Text` 値を更新してタグ **`H1`** と **`u`** タグを使用します。

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   http://localhost:3000に移動して、効果を確認し [ます](http://localhost:3000) 。

   ![更新されたテキストモデル](./assets/map-components/updated-text-model.png)

   プロパティを `richText` true **/****** falseの間で切り替えて、実行中のレンダリングロジックを確認してください。

8. Inspect `Text.scss` は `ui.frontend/src/components/Text/Text.scss`.

   このファイルは、この章のスターターコードベースによって追加され、前の章で追加された [Sass](https://sass-lang.com/) 機能を使用します。 参照元の変数に注意してくだ `ui.frontend/src/styles/_variables.scss`さい。

## 画像コンポーネントの作成

次に、AEM `Image` Imageコンポーネントにマッピングされる [Reactコンポーネントを作成します](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html)。 この `Image` コンポーネントは、 **content** componentの別の例です。

### InspectJSON

SPAコードにジャンプする前に、AEMが提供するJSONモデルを調べます。

1. コアコンポーネントライブラリの [画像の例に移動します](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)。

   ![Image CoreコンポーネントJSON](./assets/map-components/image-json.png)

   SPAコンポー `src`ネントの設定に、、、のプロパティ `alt``title``Image` が使用されます。

   >[!NOTE]
   >
   > 開発者がアダプティブコンポーネントと遅延読み込みコンポーネントを作成できるように、公開されているその他の画像プロパティ(`lazyEnabled`、 `widths`)があります。 このチュートリアルで作成するコンポーネントは簡単で、これらの高度なプロパティは **使用しません** 。

2. IDEに戻り、atを開き `mock.model.json` ま `ui.frontend/public/mock-content/mock.model.json`す。 これはプロジェクトの新しいコンポーネントなので、画像JSONを「モック」する必要があります。

   ～line 70で、 `image` モデルにJSONエントリを追加し(2番目のカンマの後のカンマを忘れないでください `,` )、 `text_23828680``:itemsOrder` 配列を更新します。

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   このプロジェクトには、 `/mock-content/adobestock-140634652.jpeg` webpack-dev-server **で使用するサンプルイメージが含まれています**。

   ここで完全な [mock.model.jsonを表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)。

### 画像コンポーネントの実装

1. 次に、の下にという名前の新しいフォルダ `Image` ーを作成し `ui.frontend/src/components`ます。
2. Beneath the `Image` folder create a new file named `Image.js`.

   ![Image.jsファイル](./assets/map-components/image-js-file.png)

3. 次追加の `import` 文を `Image.js`次に示します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 次に、を追加し `ImageEditConfig` て、AEMでプレースホルダーを表示するタイミングを決定します。

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   プ `src` ロパティが設定されていない場合は、プレースホルダが表示されます。

5. 次に、 `Image` クラスを実装します。

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   上記のコードは、propに基づ `<img>` いてレンダリングされ、JSONモデルによって `src`渡され `alt``title` る、およびに基づいてレンダリングされます。

6. Reactコンポ追加ーネントをAEMコンポーネントにマップするコード： `MapTo`

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   文字列は、次の場所にあるAEMコンポーネントの場所に `wknd-spa-react/components/image` 対応し `ui.apps` ています。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. 同じディレクトリ内にという名前 `Image.scss` の新しいファイルを作成し、次を追加します。

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. で、文の下の上部にあるファイルに参照を `Image.js` 追加し `import` ます。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   完成した [Image.jsは、ここで表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)。

9. ファイルを開き、新しいコンポー `ui.frontend/src/components/import-components.js``Image` ネントに参照を追加します。

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. まだ起動していない場合は、 **webpack-dev-serverを開始します**。 http://localhost:3000 [に移動すると](http://localhost:3000) 、画像レンダリングが表示されます。

   ![画像が](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **ボーナスの課題**:の値を画像の下にキャプション `Image.js` として表示するため `this.props.title` の新しいメソッドをに実装します。

## AEMでのポリシーの更新

コンポー `Image` ネントは、 **webpack-dev-serverでのみ表示されます**。 次に、更新したSPAをAEMにデプロイし、テンプレートポリシーを更新します。

1. Webpack- **dev-server** を停止し、プロジェクトのルートから、Mavenスキルを使用してAEMに変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM開始画面で、 **ツール** / **テンプレート** / **[WKND SPA Reactに移動します](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**。

   「 **SPA Page**」を選択して編集します。

   ![SPAページテンプレートの編集](./assets/map-components/edit-spa-page-template.png)

3. レイ **アウトコンテナを選択し** 、「 **ポリシー** 」アイコンをクリックして、ポリシーを編集します。

   ![レイアウトコンテナポリシー](./assets/map-components/layout-container-policy.png)

4. 「 **許可されているコンポーネント** / **WKND SPA React - Content** 」で、 **** 画像コンポーネントを確認します。

   ![画像コンポーネントが選択されました](./assets/map-components/check-image-component.png)

   「 **デフォルトのコンポーネント** / **マッピング** 」で **、「** 画像 — WKND SPA React — コンテンツ」コンポーネントを選択します。

   ![デフォルトコンポーネントの設定](./assets/map-components/default-components.png)

   の **MIMEタイプを入力し**`image/*`ます。

   「 **完了** 」をクリックして、ポリシーの更新を保存します。

5. レイ **アウトコンテナで** 、「 **Text** 」コンポーネントのポリシー **** アイコンをクリックします。

   ![テキストコンポーネントポリシーアイコン](./assets/map-components/edit-text-policy.png)

   Create a new policy named **WKND SPA Text**. 「 **プラグイン** / **書式設定** /すべてのチェックボックスをオンにして、追加の書式設定オプションを有効にします。

   ![RTE書式を有効にする](assets/map-components/enable-formatting-rte.png)

   「 **プラグイン** / **段落スタイル** 」で、「段落スタイルを **有効にする」チェックボックスをオンにします**。

   ![段落スタイルを有効にする](./assets/map-components/text-policy-enable-paragraphstyles.png)

   「 **完了** 」をクリックして、ポリシーの更新を保存します。

6. ホーム **ページ** http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに移動します [](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

   また、 `Text` コンポーネントを編集し、 **フルスクリーンモードで段落スタイルを追加することもできます** 。

   ![フルスクリーンリッチテキストの編集](assets/map-components/full-screen-rte.png)

7. また、 **アセットファインダーから画像をドラッグ&amp;ドロップできる必要があります**。

   ![画像のドラッグ&amp;ドロップ](./assets/map-components/drag-drop-image.gif)

8. [AEM Assetsを追加通じて独自の画像を作成するか](http://localhost:4502/assets.html/content/dam) 、標準の [WKNDリファレンスサイト用の完成したコードベースをインストールします](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKNDリファレンスサイト [には](https://github.com/adobe/aem-guides-wknd/releases/latest) 、WKND SPAで再利用できる多くの画像が含まれています。 パッケージは、 [AEM Package Managerを使用してインストールできます](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

## レイアウトコンテナのInspect

レイ **アウトコンテナのサポートは** 、AEM SPA Editor SDKによって自動的に提供されます。 名前で示される **レイアウトコンテナ**&#x200B;は **、** コンテナコンポーネントです。 コンテナコンポーネントは、 *他のコンポーネントを表すJSON構造を受け入れ、動的にインスタンス化するコンポーネントで* す。

レイアウトコンテナをさらに詳しく見てみましょう。

1. ブラウザーで、http://localhost:4502/content/wknd-spa-react/us/en.model.jsonに移動し [ます。](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSONモデルAPI — レスポンシブグリッド](./assets/map-components/responsive-grid-modeljson.png)

   レイ **アウトコンテナ** コンポーネントは、のを持ち、SPAエディタでは、のコンポーネント `sling:resourceType` と同様に、 `wcm/foundation/components/responsivegrid` プロパティを使用して認識され `:type``Text``Image` ます。

   レイ [アウトモードを使用したコンポーネントのサイズ変更と同じ機能をSPAエディタで使用できます](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 。

2. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに戻り [ます](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 その他の **画像** コンポーネントを追加追加し、「 **レイアウト** 」オプションを使用してサイズを変更してみてください。

   ![レイアウトモードを使用した画像のサイズ変更](./assets/map-components/responsive-grid-layout-change.gif)

3. JSONモデルhttp://localhost:4502/content/wknd-spa-react/us/en.model.jsonを再度開き [、JSONの一部](http://localhost:4502/content/wknd-spa-react/us/en.model.json)`columnClassNames` としての値を確認します。

   ![列クラス名](./assets/map-components/responsive-grid-classnames.png)

   クラス名は、12列のグリッドに基づいて4列幅のコンポーネントである必要があることを `aem-GridColumn--default--4` 示します。 レス [ポンシブグリッドについて詳しくは、こちらを参照してください](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. IDEに戻り、 `ui.apps` モジュール内に、に定義されたクライアント側ライブラリがあり `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`ます。 Open the file `less/grid.less`.

   このファイルは、`default`レイアウトコンテナで使用されるブレークポイント( `tablet`、 `phone`および **)を決定します**。 このファイルは、プロジェクト仕様に合わせてカスタマイズすることを目的としています。 現在、ブレークポイントはと `1200px` に設定されて `650px`います。

5. コンポーネントのレスポンシブ機能と更新されたリッチテキストポリシーを使用して、次のような表示を作成できる `Text` はずです。

   ![チャプターサンプル最終オーサリング](./assets/map-components/final-page.png)

## バリデーターが{#congratulations}

これで、SPAコンポーネントをAEMコンポーネントにマップする方法を学習し、新しい `Image` コンポーネントを実装しました。 また、 **レイアウトコンテナのレスポンシブ機能を調べる機会もありました**。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/map-components-solution`ます。

### 次の手順 {#next-steps}

[ナビゲーションとルーティング](navigation-routing.md) - SPAエディターSDKを使用してAEMページにマッピングすることで、SPA内の複数の表示をサポートする方法を学びます。 ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。

## ボーナス：構成をソース管理に永続的に {#bonus}

多くの場合、特にAEMプロジェクトの最初に、テンプレートや関連するコンテンツポリシーなどの設定を保持してソース管理を行うことが重要です。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性を確保できます。 プロジェクトが一定の成熟度に達したら、テンプレートの管理方法をパワーユーザーの特別なグループに変えることができます。

次のいくつかの手順は、Visual Studio Code IDEと [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) を使用して行われますが **、AEMのローカルインスタンスからコンテンツを** 取り込んだり **** 読み込んだりするように設定した任意のツールとIDEを使用して行うことができます。

1. Visual StudioコードIDEで、Marketplace拡張機能を使用して **VSCode AEM Sync** がインストールされていることを確認します。

   ![VSCode AEM同期](./assets/map-components/vscode-aem-sync.png)

2. プロジェクトエクスプローラで **ui.content** モジュールを展開し、に移動し `/conf/wknd-spa-react/settings/wcm/templates`ます。

3. **フォルダーを右クリックし** 、 `templates` 「AEM Server ****:

   ![VSCodeインポートテンプレート](./assets/map-components/import-aem-servervscode.png)

4. コンテンツを読み込む手順を繰り返しますが、にある **ポリシー** フォルダーを選択し `/conf/wknd-spa-react/settings/wcm/templates/policies`ます。

5. フ `filter.xml` ァイルをInspectに配置し `ui.content/src/main/content/META-INF/vault/filter.xml`ます。

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   この `filter.xml` ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルター `mode="merge"` に、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示すが、 コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントでコンテンツが上書きされないこ **とが重要です** 。 フィルタ要素の操作の詳細については、 [FileVaultのドキュメント](https://jackrabbit.apache.org/filevault/filter.html) を参照してください。

   各モジュール `ui.content/src/main/content/META-INF/vault/filter.xml` で管理され `ui.apps/src/main/content/META-INF/vault/filter.xml` る各ノードを比較し、理解します。
