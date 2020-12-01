---
title: SPAコンポーネントのAEMコンポーネントへのマッピング | AEM SPA EditorとReactの使い始めに
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


# SPAコンポーネントをAEMコンポーネントにマップ{#map-components}

AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントを動的に更新できます。

この章では、AEM JSONモデルAPIについて詳しく説明し、AEMコンポーネントによって公開されたJSONコンテンツを、propとして自動的にReactコンポーネントに挿入する方法について説明します。

## 目的

1. AEMコンポーネントをSPAコンポーネントにマップする方法を学びます。
2. **コンテナ**&#x200B;コンポーネントと&#x200B;**コンテンツ**&#x200B;コンポーネントの違いを理解します。
3. 既存のAEMコンポーネントにマッピングする新しいReactコンポーネントを作成します。

## 作成する内容

この章では、提供された`Text` SPAコンポーネントがAEM `Text`コンポーネントにどのようにマッピングされるかを調べます。 新しい`Image` SPAコンポーネントが作成され、SPAで使用してAEMで作成できます。 **レイアウトコンテナ**&#x200B;および&#x200B;**テンプレートエディター**&#x200B;ポリシーの初期設定の機能は、外観が少し変化する表示の作成にも使用されます。

![チャプターサンプル最終オーサリング](./assets/map-components/final-page.png)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

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

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)に常に表示できます。また、ブランチ`React/map-components-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## マッピング手法

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマップすることです。 AEMコンポーネントでは、サーバーサイドで実行し、JSONモデルAPIの一部としてコンテンツを書き出します。 JSONコンテンツは、ブラウザーでクライアント側を実行するSPAで使用されます。 SPAコンポーネントとAEMコンポーネント間の1:1マッピングが作成されます。

![AEMコンポーネントとReactコンポーネントのマッピングの概要](./assets/map-components/high-level-approach.png)

*AEMコンポーネントとReactコンポーネントのマッピングの概要*

## テキストコンポーネントのInspect

[AEMプロジェクトのアーキタイプ](https://github.com/adobe/aem-project-archetype)には、AEM [テキストコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/text.html)にマップされる`Text`コンポーネントが用意されています。 これは、AEMの&#x200B;**content**&#x200B;コンポーネントの例で、の&#x200B;*content*&#x200B;をレンダリングします。

コンポーネントの動作を見てみましょう。

### JSONモデルのInspect

1. SPAコードにジャンプする前に、AEMが提供するJSONモデルを理解することが重要です。 [コアコンポーネントライブラリ](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)に移動し、テキストコンポーネントのページを表示します。 コアコンポーネントライブラリは、すべてのAEMコアコンポーネントの例を提供します。
2. 次のいずれかの例の&#x200B;**JSON**&#x200B;タブを選択します。

   ![テキストJSONモデル](./assets/map-components/text-json.png)

   次の3つのプロパティが表示されます。`text`、`richText`、`:type`。

   `:type` は、AEMコンポーネントの `sling:resourceType` （またはパス）をリストする予約済みのプロパティです。`:type`の値は、AEMコンポーネントをSPAコンポーネントにマップする際に使用する値です。

   `text` および `richText` は、SPAコンポーネントに公開される追加のプロパティです。

### テキストコンポーネントのInspect化

1. 新しいターミナルを開き、プロジェクト内の`ui.frontend`フォルダーに移動します。 `npm install`を実行し、`npm start`を実行して&#x200B;**webpack-dev-server**&#x200B;を開始します。

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   `ui.frontend`モジュールは現在、[mock JSONモデル](./integrate-spa.md#mock-json)を使用するように設定されています。

2. 新しいブラウザウィンドウが開いて[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)が開きます。

   ![モックコンテンツを含むWebpackデベロッパーサーバー](./assets/map-components/initial-start.png)

3. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 `ui.frontend`モジュールを展開し、`ui.frontend/src/components/Text/Text.js`の下の`Text.js`ファイルを開きます。

   ![Text.jsリアクションコンポーネントのソースコード](./assets/map-components/vscode-ide-text-js.png)

4. 最初に調べる領域は、～行40の`class Text`です。

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

   `Text` は、標準的なReactコンポーネントです。コンポーネントは`this.props.richText`を使用して、レンダリングするコンテンツをリッチテキストにするかプレーンテキストにするかを決定します。 実際に使用される「コンテンツ」は`this.props.text`から来ています。 潜在的なXSS攻撃を防ぐために、`DOMPurify`を介してリッチテキストをエスケープしてから、[dangerelySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)を使用してコンテンツをレンダリングします。 演習の前のJSONモデルから`richText`プロパティと`text`プロパティを呼び出します。

5. 次に`TextEditConfig`を見て下さい。

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上記のコードは、AEM作成者環境でプレースホルダーをレンダリングするタイミングを決定する役割を持ちます。 `isEmpty`メソッドが&#x200B;**true**&#x200B;を返す場合は、プレースホルダーがレンダリングされます。

6. 最後に、～line 62の`MapTo`呼び出しを見てみましょう。

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` は、AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)で提供されます。パス`wknd-spa-react/components/text`は、AEMコンポーネントの`sling:resourceType`を表します。 このパスは、前に確認したJSONモデルによって公開された`:type`と一致します。 `MapTo` は、JSONモデルの応答を解析し、正しい値をSPAコンポーネントに渡す処理 `props` を行います。

   AEM `Text`コンポーネント定義は`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`にあります。

7. `ui.frontend/public/mock-content/mock.model.json`の`mock.model.json`ファイルを変更して、実験してみてください。 ～line 62で、最初の`Text`値を更新して&#x200B;**`H1`**&#x200B;タグと&#x200B;**`u`**&#x200B;タグを使用します。

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   [http://localhost:3000](http://localhost:3000)に移動して、効果を確認します。

   ![更新されたテキストモデル](./assets/map-components/updated-text-model.png)

   `richText`プロパティを&#x200B;**true** / **false**&#x200B;の間で切り替えて、実行中のレンダリングロジックを確認してください。

8. Inspect`Text.scss`、`ui.frontend/src/components/Text/Text.scss`。

   このファイルは、この章のスターターコードベースによって追加され、前の章で追加された[Sass](https://sass-lang.com/)機能を使用します。 `ui.frontend/src/styles/_variables.scss`から参照される変数をメモしておきます。

## 画像コンポーネントの作成

次に、AEM [画像コンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html)にマッピングされる`Image`リアクトコンポーネントを作成します。 `Image`コンポーネントは、**content**&#x200B;コンポーネントの別の例です。

### InspectJSON

SPAコードにジャンプする前に、AEMが提供するJSONモデルを調べます。

1. コアコンポーネントライブラリ](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)の[イメージサンプルに移動します。

   ![Image CoreコンポーネントJSON](./assets/map-components/image-json.png)

   `src`、`alt`、および`title`のプロパティは、SPA `Image`コンポーネントの入力に使用されます。

   >[!NOTE]
   >
   > 開発者がアダプティブおよび遅延読み込みコンポーネントを作成できるように、公開されている他の画像プロパティ(`lazyEnabled`、`widths`)があります。 このチュートリアルで作成されるコンポーネントは単純で、****&#x200B;はこれらの高度なプロパティを使用しません。

2. IDEに戻り、`ui.frontend/public/mock-content/mock.model.json`の`mock.model.json`を開きます。 これはプロジェクトの新しいコンポーネントなので、画像JSONを「モック」する必要があります。

   ～line 70で、`image`モデルのJSONエントリを追加し（2番目の`text_23828680`の後ろのカンマ`,`を忘れないでください）、`:itemsOrder`配列を更新します。

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

   このプロジェクトには、**webpack-dev-server**&#x200B;で使用するサンプルイメージが`/mock-content/adobestock-140634652.jpeg`に含まれています。

   [mock.model.jsonの完全な表示はここ](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)で行うことができます。

### 画像コンポーネントの実装

1. 次に、`ui.frontend/src/components`の下に`Image`という名前の新しいフォルダーを作成します。
2. `Image`フォルダーの下に、`Image.js`という名前の新しいファイルを作成します。

   ![Image.jsファイル](./assets/map-components/image-js-file.png)

3. 追加次の`import`文を`Image.js`に渡します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 次に`ImageEditConfig`を追加し、AEMでプレースホルダーを表示するタイミングを決定します。

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   `src`プロパティが設定されていない場合は、プレースホルダが表示されます。

5. 次に`Image`クラスを実装します。

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

   上記のコードは、JSONモデルによって渡されるprop `src`、`alt`および`title`に基づいて`<img>`をレンダリングします。

6. Reactコンポー追加ネントをAEMコンポーネントにマップする`MapTo`コード：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   文字列`wknd-spa-react/components/image`は、次の場所にある`ui.apps`のAEMコンポーネントの場所に対応しています。`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

7. 同じディレクトリに`Image.scss`という名前の新しいファイルを作成し、次の内容を追加します。

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. `Image.js`で、ファイルの上部の`import`文の下に参照を追加します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   完了した[Image.jsを表示するには、ここ](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)をクリックします。

9. ファイル`ui.frontend/src/components/import-components.js`を開き、新しい`Image`コンポーネントに参照を追加します。

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. まだ起動していない場合は、**webpack-dev-server**&#x200B;を開始します。 [http://localhost:3000](http://localhost:3000)に移動すると、イメージレンダリングが表示されます。

   ![画像が](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **ボーナスの課題**:の値を画像の下にキャプション `Image.js` として表示す `this.props.title` るための新しいメソッドをに実装します。

## AEMでのポリシーの更新

`Image`コンポーネントは、**webpack-dev-server**&#x200B;にのみ表示されます。 次に、更新したSPAをAEMにデプロイし、テンプレートポリシーを更新します。

1. **webpack-dev-server**&#x200B;を停止し、プロジェクトのルートから、Mavenのスキルを使用してAEMに変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM開始画面で、**ツール** > **テンプレート** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**&#x200B;に移動します。

   **SPAページ**&#x200B;を選択して編集します。

   ![SPAページテンプレートの編集](./assets/map-components/edit-spa-page-template.png)

3. **レイアウトコンテナ**&#x200B;を選択し、**ポリシー**&#x200B;アイコンをクリックして、ポリシーを編集します。

   ![レイアウトコンテナポリシー](./assets/map-components/layout-container-policy.png)

4. **許可されているコンポーネント** > **WKND SPA React - Content**&#x200B;の下で、**イメージ**&#x200B;コンポーネントを確認します。

   ![画像コンポーネントが選択されました](./assets/map-components/check-image-component.png)

   **デフォルトコンポーネント** > **追加マッピング**&#x200B;の下で、**画像 — WKND SPA React — コンテンツ**&#x200B;コンポーネントを選択します。

   ![デフォルトコンポーネントの設定](./assets/map-components/default-components.png)

   `image/*`の&#x200B;**MIMEタイプ**&#x200B;を入力します。

   「**完了**」をクリックして、ポリシーの更新を保存します。

5. **レイアウトコンテナ**&#x200B;で、**テキスト**&#x200B;コンポーネントの&#x200B;**ポリシー**&#x200B;アイコンをクリックします。

   ![テキストコンポーネントポリシーアイコン](./assets/map-components/edit-text-policy.png)

   **WKND SPA Text**&#x200B;という名前の新しいポリシーを作成します。 「**プラグイン**/**書式設定**」の下のすべてのチェックボックスをオンにして、追加の書式設定オプションを有効にします。

   ![RTE書式を有効にする](assets/map-components/enable-formatting-rte.png)

   **プラグイン**/**段落スタイル**&#x200B;の下で、**段落スタイル**&#x200B;を有効にするチェックボックスをオンにします。

   ![段落スタイルを有効にする](./assets/map-components/text-policy-enable-paragraphstyles.png)

   「**完了**」をクリックして、ポリシーの更新を保存します。

6. **ホームページ** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。

   また、`Text`コンポーネントを編集し、**フルスクリーン**&#x200B;モードで段落スタイルを追加することもできます。

   ![フルスクリーンリッチテキストの編集](assets/map-components/full-screen-rte.png)

7. また、**アセットファインダー**&#x200B;から画像をドラッグ&amp;ドロップすることもできます。

   ![画像のドラッグ&amp;ドロップ](./assets/map-components/drag-drop-image.gif)

8. &lt;a0/追加>AEM Assets[を通して自分のイメージを作るか、標準の](http://localhost:4502/assets.html/content/dam)WKNDリファレンスサイト[の完成したコードベースをインストールします。 ](https://github.com/adobe/aem-guides-wknd/releases/latest)[WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)には、WKND SPAで再利用できる多くの画像が含まれています。 パッケージは、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

## レイアウトコンテナのInspect

**レイアウトコンテナ**&#x200B;は、AEM SPAエディタSDKによって自動的に提供されます。 **レイアウトコンテナ**&#x200B;は、名前で示されているとおり、**コンテナ**&#x200B;コンポーネントです。 コンテナコンポーネントは、*他の*&#x200B;コンポーネントを表すJSON構造を受け入れ、動的にインスタンス化するコンポーネントです。

レイアウトコンテナをさらに詳しく見てみましょう。

1. ブラウザーで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)に移動します。

   ![JSONモデルAPI — レスポンシブグリッド](./assets/map-components/responsive-grid-modeljson.png)

   **レイアウトコンテナ**&#x200B;コンポーネントは`wcm/foundation/components/responsivegrid`の`sling:resourceType`を持ち、`Text`コンポーネントと`Image`コンポーネントのように、`:type`プロパティを使用してSPAエディターで認識されます。

   [レイアウトモード](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)を使用してコンポーネントのサイズを変更する場合と同じ機能をSPAエディタで使用できます。

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に戻ります。 追加の追加&#x200B;**画像**&#x200B;コンポーネントを&#x200B;**レイアウト**&#x200B;オプションを使用してサイズ変更を試みます。

   ![レイアウトモードを使用した画像のサイズ変更](./assets/map-components/responsive-grid-layout-change.gif)

3. JSONモデル[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を再度開き、`columnClassNames`をJSONの一部として観察します。

   ![列クラス名](./assets/map-components/responsive-grid-classnames.png)

   クラス名`aem-GridColumn--default--4`は、12列のグリッドに基づく4列幅のコンポーネントである必要があることを示します。 [レスポンシブグリッドの詳細は、](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)を参照してください。

4. IDEに戻り、`ui.apps`モジュールに`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`に定義されたクライアント側ライブラリがあります。 `less/grid.less` ファイルを開きます。

   このファイルは、**レイアウトコンテナ**&#x200B;で使用されるブレークポイント(`default`、`tablet`、`phone`)を決定します。 このファイルは、プロジェクト仕様に合わせてカスタマイズすることを目的としています。 現在、ブレークポイントは`1200px`と`650px`に設定されています。

5. `Text`コンポーネントのレスポンシブ機能と更新されたリッチテキストポリシーを使用して、次のような表示を作成できるはずです。

   ![チャプターサンプル最終オーサリング](./assets/map-components/final-page.png)

## バリデーターが{#congratulations}

SPAコンポーネントをAEMコンポーネントにマップする方法を学び、新しい`Image`コンポーネントを実装しました。 **レイアウトコンテナ**&#x200B;のレスポンシブ機能を調べる機会もあります。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)に常に表示できます。また、ブランチ`React/map-components-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

### 次の手順 {#next-steps}

[ナビゲーションとルーティング](navigation-routing.md) - SPA Editor SDKを使用してAEMページにマッピングすることで、SPAの複数の表示をどのようにサポートできるかを学びます。ダイナミックナビゲーションはReact Routerを使用して実装され、既存のヘッダコンポーネントに追加されます。

## ボーナス — ソース管理{#bonus}に対する構成の永続化

多くの場合、特にAEMプロジェクトの最初に、テンプレートや関連するコンテンツポリシーなどの設定を保持してソース管理を行うことが重要です。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性を確保できます。 プロジェクトが一定の成熟度に達したら、テンプレートの管理方法をパワーユーザーの特別なグループに変えることができます。

次の手順は、Visual StudioコードIDEと[VSCode AEM同期](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)を使用して行いますが、AEMのローカルインスタンスから&#x200B;**pull**&#x200B;または&#x200B;**import**&#x200B;に設定した任意のツールとIDEを使用して行う場合があります。

1. Visual StudioコードIDEで、Marketplaceの拡張機能を介して&#x200B;**VSCode AEM Sync**&#x200B;がインストールされていることを確認します。

   ![VSCode AEM同期](./assets/map-components/vscode-aem-sync.png)

2. プロジェクトエクスプローラーで&#x200B;**ui.content**&#x200B;モジュールを展開し、`/conf/wknd-spa-react/settings/wcm/templates`に移動します。

3. **フォルダーを右** クリックし、「AEM Server `templates` から **読み込み**」を選択します。

   ![VSCodeインポートテンプレート](./assets/map-components/import-aem-servervscode.png)

4. コンテンツを読み込む手順を繰り返しますが、`/conf/wknd-spa-react/settings/wcm/templates/policies`にある&#x200B;**ポリシー**&#x200B;フォルダーを選択します。

5. `filter.xml`ファイルをInspect`ui.content/src/main/content/META-INF/vault/filter.xml`に置きます。

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

   `filter.xml`ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルターの`mode="merge"`は、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示しています。 コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントではコンテンツを&#x200B;**上書きしない**&#x200B;ことが重要です。 フィルタ要素の操作の詳細については、[FileVaultドキュメント](https://jackrabbit.apache.org/filevault/filter.html)を参照してください。

   `ui.content/src/main/content/META-INF/vault/filter.xml`と`ui.apps/src/main/content/META-INF/vault/filter.xml`を比較して、各モジュールで管理される異なるノードを理解します。
