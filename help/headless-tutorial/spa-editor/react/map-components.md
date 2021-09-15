---
title: SPAコンポーネントのAEMコンポーネントへのマッピング | AEM SPA EditorとReactの概要
description: AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、AEM SPA Editor内で、従来のAEMオーサリングと同様に、SPAコンポーネントを動的に更新できます。 また、標準搭載のAEM Reactコアコンポーネントの使用方法についても説明します。
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2263'
ht-degree: 2%

---

# SPAコンポーネントのAEMコンポーネントへのマッピング {#map-components}

AEM SPA Editor JS SDKを使用して、ReactコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、AEM SPA Editor内で、従来のAEMオーサリングと同様に、SPAコンポーネントを動的に更新できます。

この章では、AEM JSONモデルAPIについて詳しく説明し、AEMコンポーネントによって公開されたJSONコンテンツをpropとしてReactコンポーネントに自動的に挿入する方法について説明します。

## 目的

1. AEMコンポーネントをSPAコンポーネントにマッピングする方法について説明します。
1. Inspectでは、AEMから渡されたダイナミックプロパティをReactコンポーネントで使用します。
1. 標準搭載の[React AEMコアコンポーネント](https://github.com/adobe/aem-react-core-wcm-components-examples)の使用方法を説明します。

## 作成する内容

この章では、指定された`Text` SPAコンポーネントがAEM `Text`コンポーネントにどのようにマッピングされているかを調べます。 `Image` SPAコンポーネントなどのReactコアコンポーネントは、SPAで使用され、AEMでオーサリングされます。 **レイアウトコンテナ**&#x200B;および&#x200B;**テンプレートエディター**&#x200B;ポリシーの初期設定済み機能も使用して、外観が少し変化するビューを作成します。

![チャプターサンプルの最終オーサリング](./assets/map-components/final-page.png)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。 この章は、 SPA](integrate-spa.md)の統合の章の続きですが、SPA対応のAEMプロジェクトが必要な場合は、この章の続きに従ってください。[

## マッピング方法

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマッピングすることです。 AEMコンポーネントを使用して、サーバー側で実行し、JSONモデルAPIの一部としてコンテンツを書き出す。 JSONコンテンツは、ブラウザーでクライアント側を実行するSPAで使用されます。 SPAコンポーネントとAEMコンポーネントの間に1対1のマッピングが作成されます。

![AEMコンポーネントとReactコンポーネントのマッピングの概要](./assets/map-components/high-level-approach.png)

*AEMコンポーネントとReactコンポーネントのマッピングの概要*

## Inspect the Text Component

[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)は、AEM [テキストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)にマッピングされる`Text`コンポーネントを提供します。 これは、AEMから&#x200B;*content*&#x200B;をレンダリングする&#x200B;**content**&#x200B;コンポーネントの例です。

コンポーネントの動作を見てみましょう。

### JSONモデルのInspect

1. SPAのコードを調べる前に、AEMが提供するJSONモデルを理解しておくことが重要です。 [コアコンポーネントライブラリ](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)に移動し、テキストコンポーネントのページを表示します。 コアコンポーネントライブラリは、すべてのAEMコアコンポーネントの例を提供しています。
1. 次のいずれかの例の「**JSON**」タブを選択します。

   ![テキストJSONモデル](./assets/map-components/text-json.png)

   次の3つのプロパティが表示されます。`text`、`richText`、および`:type`。

   `:type` は、AEMコンポーネントの(また `sling:resourceType` はパス)をリストする予約済みプロパティです。`:type`の値は、AEMコンポーネントをSPAコンポーネントにマッピングするために使用されます。

   `text` と `richText` は、SPAコンポーネントに公開される追加のプロパティです。

1. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)でJSON出力を表示します。 次のようなエントリが見つかるはずです。

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### テキストSPAコンポーネントのInspect

1. 任意のIDEで、SPAのAEMプロジェクトを開きます。 `ui.frontend`モジュールを展開し、`ui.frontend/src/components/Text/Text.js`の下の`Text.js`ファイルを開きます。

1. 最初に調べるのは、40行目(`class Text`)です。

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

   `Text` は、標準のReactコンポーネントです。コンポーネントは、`this.props.richText`を使用して、レンダリングするコンテンツをリッチテキストにするかプレーンテキストにするかを決定します。 実際に使用される「コンテンツ」は`this.props.text`から取得されます。

   XSS攻撃の可能性を回避するために、リッチテキストは`DOMPurify`経由でエスケープされてから、[dangerlySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)を使用してコンテンツをレンダリングします。 この演習の前のJSONモデルから`richText`および`text`プロパティを呼び出します。

1. 次に、～29行目の`TextEditConfig`を見てみましょう。

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上記のコードは、AEMオーサー環境でプレースホルダーをレンダリングするタイミングを決定する役割を果たします。 `isEmpty`メソッドが&#x200B;**true**&#x200B;を返す場合は、プレースホルダーがレンダリングされます。

1. 最後に、 ～62行目の`MapTo`呼び出しを見てみましょう。

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` は、AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)で提供されます。パス`wknd-spa-react/components/text`は、AEMコンポーネントの`sling:resourceType`を表します。 このパスは、前に確認したJSONモデルで公開された`:type`と一致します。 `MapTo` は、JSONモデルの応答を解析し、正しい値をSPAコンポーネントに渡 `props` す処理を行います。

   AEMの`Text`コンポーネント定義は`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`にあります。

## Reactコアコンポーネントの使用

[AEM WCMコンポーネント — Reactコア実](https://github.com/adobe/aem-react-core-wcm-components-base) 装および [AEM WCMコンポーネント — Spaエディター — Reactコア実装](https://github.com/adobe/aem-react-core-wcm-components-spa)。これらは、すぐに使用できるAEMコンポーネントにマッピングされる、再利用可能なUIコンポーネントのセットです。 ほとんどのプロジェクトでは、これらのコンポーネントを独自の実装の出発点として再利用できます。

1. プロジェクトコードで、`import-components.js` (`ui.frontend/src/components`)にあるファイルを開きます。
このファイルは、AEMコンポーネントにマッピングされているすべてのSPAコンポーネントを読み込みます。 SPA Editor実装の動的な特性を考慮し、作成可能なAEMコンポーネントに結び付けられているすべてのSPAコンポーネントを明示的に参照する必要があります。 これにより、AEM作成者は、アプリケーション内の任意の場所でコンポーネントを使用するよう選択できます。
1. 次のimport文には、プロジェクトで書き込まれるSPAコンポーネントが含まれます。

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. `@adobe/aem-core-components-react-spa`と`@adobe/aem-core-components-react-base`の他の`imports`が複数あります。 これらはReactコアコンポーネントを読み込み、現在のプロジェクトで使用できるようにします。 前述の`Text`コンポーネントの例と同様に、 `MapTo`を使用して、これらのコンポーネントがプロジェクト固有のAEMコンポーネントにマッピングされます。

### AEM Policiesの更新

ポリシーは、AEMテンプレートの機能で、開発者とパワーユーザーは、使用可能なコンポーネントを詳細に制御できます。 ReactコアコンポーネントはSPAコードに含まれていますが、アプリケーションで使用する前に、ポリシーを使用して有効にする必要があります。

1. AEM Start画面で、**Tools** > **Templates** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**&#x200B;に移動します。

1. **SPA Page**&#x200B;テンプレートを選択して開き、編集します。

1. **レイアウトコンテナ**&#x200B;を選択し、**ポリシー**&#x200B;アイコンをクリックしてポリシーを編集します。

   ![レイアウトコンテナポリシー](assets/map-components/edit-spa-page-template.png)

1. **許可されているコンポーネント** > **WKND SPA React - Content** >で、**画像**、**ティーザー**&#x200B;および&#x200B;**タイトル**&#x200B;を確認します。

   ![使用可能な更新済みコンポーネント](assets/map-components/update-components-available.png)

   **Default Components** > **Add mapping**&#x200B;を選択し、**Image - WKND SPA React - Content**&#x200B;コンポーネントを選択します。

   ![デフォルトコンポーネントの設定](./assets/map-components/default-components.png)

   **MIMEタイプ**&#x200B;を`image/*`と入力します。

   「**完了**」をクリックして、ポリシーの更新を保存します。

1. **レイアウトコンテナ**&#x200B;で、**テキスト**&#x200B;コンポーネントの&#x200B;**ポリシー**&#x200B;アイコンをクリックします。

   **WKND SPA Text**&#x200B;という名前の新しいポリシーを作成します。 「**プラグイン** / **書式** 」で、すべてのボックスをオンにして追加の書式設定オプションを有効にします。

   ![RTEフォーマットの有効化](assets/map-components/enable-formatting-rte.png)

   **プラグイン** > **段落スタイル**&#x200B;で、「**段落スタイル**&#x200B;を有効にする」チェックボックスをオンにします。

   ![段落スタイルを有効にする](./assets/map-components/text-policy-enable-paragraphstyles.png)

   「**完了**」をクリックして、ポリシーの更新を保存します。

### 作成者コンテンツ

1. **ホームページ** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。

1. これで、ページ上の追加のコンポーネント&#x200B;**画像**、**ティーザー**、**タイトル**&#x200B;を使用できるようになります。

   ![追加のコンポーネント](assets/map-components/additional-components.png)

1. また、`Text`コンポーネントを編集し、**フルスクリーン**&#x200B;モードで段落スタイルを追加することもできます。

   ![全画面表示のリッチテキスト編集](assets/map-components/full-screen-rte.png)

1. また、**アセットファインダー**&#x200B;から画像をドラッグ&amp;ドロップすることもできます。

   ![画像のドラッグ&amp;ドロップ](assets/map-components/drag-drop-image.png)

1. **タイトル**&#x200B;および&#x200B;**ティーザー**&#x200B;コンポーネントを使用したエクスペリエンス。

1. [AEM Assets](http://localhost:4502/assets.html/content/dam)を使用して独自の画像を追加するか、標準の[WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)の完成したコードベースをインストールします。 [WKND参照サイト](https://github.com/adobe/aem-guides-wknd/releases/latest)には、WKND SPAで再利用できる多くの画像が含まれています。 パッケージは、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![パッケージマネージャーによるwknd.allのインストール](./assets/map-components/package-manager-wknd-all.png)

## Inspect the Layout Container

**レイアウトコンテナ**&#x200B;のサポートは、AEM SPA Editor SDKによって自動的に提供されます。 **レイアウトコンテナ**&#x200B;は、名前で示されているように、 **コンテナ**&#x200B;コンポーネントです。 コンテナコンポーネントは、*他の*&#x200B;コンポーネントを表すJSON構造を受け入れ、動的にインスタンス化するコンポーネントです。

ここでは、レイアウトコンテナをさらに詳しく調べます。

1. ブラウザーで、[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)に移動します。

   ![JSONモデルAPI — レスポンシブグリッド](./assets/map-components/responsive-grid-modeljson.png)

   **レイアウトコンテナ**&#x200B;コンポーネントは`wcm/foundation/components/responsivegrid`の`sling:resourceType`を持ち、`Text`コンポーネントと`Image`コンポーネントと同様に、`:type`プロパティを使用してSPAエディターで認識されます。

   [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)を使用してコンポーネントのサイズを変更する場合と同じ機能をSPA Editorで使用できます。

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に戻ります。 **画像**&#x200B;コンポーネントを追加し、**レイアウト**&#x200B;オプションを使用してサイズ変更を試みます。

   ![レイアウトモードを使用した画像のサイズ変更](./assets/map-components/responsive-grid-layout-change.gif)

3. JSONモデル[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を再度開き、JSONの一部として`columnClassNames`を観察します。

   ![列クラス名](./assets/map-components/responsive-grid-classnames.png)

   クラス名`aem-GridColumn--default--4`は、12列のグリッドに基づいて、幅が4列であるコンポーネントを示します。 [レスポンシブグリッドの詳細については、](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)を参照してください。

4. IDEに戻り、`ui.apps`モジュール内に`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`に定義されたクライアント側ライブラリがあります。 `less/grid.less` ファイルを開きます。

   このファイルは、**レイアウトコンテナ**&#x200B;で使用されるブレークポイント(`default`、`tablet`、`phone`)を決定します。 このファイルは、プロジェクト仕様に応じてカスタマイズすることを目的としています。 現在、ブレークポイントは`1200px`と`768px`に設定されています。

5. `Text`コンポーネントのレスポンシブ機能と更新されたリッチテキストポリシーを使用して、次のようなビューを作成できます。

   ![チャプターサンプルの最終オーサリング](assets/map-components/final-page.png)

## おめでとうございます。 {#congratulations}

これで、SPAコンポーネントをAEMコンポーネントにマッピングする方法と、Reactコアコンポーネントを使用した方法を学びました。 また、**レイアウトコンテナ**&#x200B;のレスポンシブ機能を調べることもできます。

### 次の手順 {#next-steps}

[ナビゲーションとルーティング](navigation-routing.md)  - SPAエディターSDKを使用してAEMページにマッピングすることで、SPAの複数のビューをサポートする方法を説明します。動的なナビゲーションは、React RouterとReactコアコンポーネントを使用して実装されます。

## （ボーナス）ソース管理に対する設定の保持 {#bonus-configs}

多くの場合、特にAEMプロジェクトの開始時に、テンプレートや関連するコンテンツポリシーなどの設定をソース管理に保持すると役立ちます。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性をさらに高めることができます。 プロジェクトがある程度の成熟度に達すると、テンプレート管理の手法を特別なパワーユーザーのグループに引き継ぐことができます。

次の手順は、Visual Studio Code IDEと[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)を使用して実行しますが、AEMのローカルインスタンスから&#x200B;**pull&lt;a3/または** import **に設定した任意のツールとIDEを使用して実行できます。**

1. Visual Studio Code IDEで、Marketplace拡張機能を使用して&#x200B;**VSCode AEM Sync**&#x200B;がインストールされていることを確認します。

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. プロジェクトエクスプローラーで、 **ui.content**&#x200B;モジュールを展開し、 `/conf/wknd-spa-react/settings/wcm/templates`に移動します。

3. **フォルダーを右** クリック `templates` し、「 AEM Serverからインポ **ート」を選択します**。

   ![VSCodeインポートテンプレート](./assets/map-components/import-aem-servervscode.png)

4. コンテンツを読み込む手順を繰り返しますが、`/conf/wknd-spa-react/settings/wcm/templates/policies`にある&#x200B;**policies**&#x200B;フォルダーを選択します。

5. `ui.content/src/main/content/META-INF/vault/filter.xml`にある`filter.xml`ファイルをInspectします。

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

   `filter.xml`ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルターの`mode="merge"`は、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示しています。 コンテンツ作成者はこれらのパスを更新する可能性があるので、コードデプロイメントではコンテンツを上書き&#x200B;**しない**&#x200B;ことが重要です。 フィルタ要素の使用について詳しくは、[FileVaultのドキュメント](https://jackrabbit.apache.org/filevault/filter.html)を参照してください。

   `ui.content/src/main/content/META-INF/vault/filter.xml`と`ui.apps/src/main/content/META-INF/vault/filter.xml`を比較して、各モジュールで管理される異なるノードを理解します。

## （ボーナス）カスタム画像コンポーネントの作成 {#bonus-image}

SPA画像コンポーネントは、Reactコアコンポーネントによって既に提供されています。 ただし、追加のプラクティスが必要な場合は、AEMの[画像コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)にマッピングする独自のReact実装を作成します。 `Image`コンポーネントは、**content**&#x200B;コンポーネントの別の例です。

### Inspect the JSON

SPAコードを調べる前に、AEMから提供されたJSONモデルを調べます。

1. コアコンポーネントライブラリの[画像の例](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)に移動します。

   ![画像コアコンポーネントのJSON](./assets/map-components/image-json.png)

   `src`、`alt`および`title`のプロパティは、SPA `Image`コンポーネントの設定に使用されます。

   >[!NOTE]
   >
   > 開発者がアダプティブな遅延読み込みコンポーネントを作成できる、その他の画像プロパティ(`lazyEnabled`、`widths`)が公開されています。 このチュートリアルで作成されるコンポーネントはシンプルで、これらの高度なプロパティは&#x200B;**使用**&#x200B;しません。

### 画像コンポーネントの実装

1. 次に、`ui.frontend/src/components`の下に`Image`という名前の新しいフォルダーを作成します。
1. `Image`フォルダーの下に、`Image.js`という名前の新しいファイルを作成します。

   ![Image.jsファイル](./assets/map-components/image-js-file.png)

1. 次の`import`文を`Image.js`に追加します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 次に、`ImageEditConfig`を追加して、AEMでプレースホルダーを表示するタイミングを指定します。

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   `src`プロパティが設定されていない場合、プレースホルダーが表示されます。

1. 次に、`Image`クラスを実装します。

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

   上記のコードは、JSONモデルによって渡されたprop `src`、`alt`および`title`に基づいて`<img>`をレンダリングします。

1. `MapTo`コードを追加して、ReactコンポーネントをAEMコンポーネントにマッピングします。

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   文字列`wknd-spa-react/components/image`は、次の場所にある`ui.apps`のAEMコンポーネントの場所に対応しています。`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. `Image.css`という名前の新しいファイルを同じディレクトリに作成し、次のコードを追加します。

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. `Image.js`内で、`import`ステートメントの下の先頭にファイルへの参照を追加します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. ファイル`ui.frontend/src/components/import-components.js`を開き、新しい`Image`コンポーネントへの参照を追加します。

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. `import-components.js`で、Reactコアコンポーネントの画像をコメントアウトします。

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   これにより、カスタムの画像コンポーネントが確実に代わりに使用されます。

1. プロジェクトのルートから、Mavenを使用してSPAコードをAEMにデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. AEMでSPAをInspectします。 ページ上の画像コンポーネントは、引き続き機能します。 Inspectレンダリング出力を見ると、Reactコアコンポーネントではなく、カスタム画像コンポーネントのマークアップが表示されます。

   *カスタム画像コンポーネントのマークアップ*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Reactコアコンポーネントの画像マークアップ*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   これは、独自のコンポーネントの拡張と実装の良い紹介です。
