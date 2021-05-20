---
title: 編集可能なコンテナコンポーネントをリモートSPAに追加
description: AEM作成者がコンポーネントをリモートSPAにドラッグ&ドロップできるように、編集可能なコンテナコンポーネントをリモートコンテナに追加する方法を説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 1%

---


# 編集可能なコンテナコンポーネント

[固定コン](./spa-fixed-component.md) ポーネントは、SPAコンテンツを柔軟にオーサリングできますが、このアプローチは厳格で、編集可能コンテンツの正確な構成を定義する開発者が必要です。作成者が例外的なエクスペリエンスを作成できるように、SPA EditorではSPAでのコンテナコンポーネントの使用をサポートしています。 コンテナコンポーネントを使用すると、作成者は、従来のAEM Sitesオーサリングと同様に、許可されたコンポーネントをコンテナにドラッグ&amp;ドロップしたり、コンポーネントをオーサリングしたりできます。

![編集可能なコンテナコンポーネント](./assets/spa-container-component/intro.png)

この章では、SPAで直接AEM Reactコアコンポーネントを使用して、作成者がリッチコンテンツエクスペリエンスを作成およびレイアウトできる、編集可能なコンテナをホームビューに追加します。

## WKNDアプリの更新

コンテナコンポーネントをホームビューに追加するには：

+ AEM React編集可能コンポーネントのResponsiveGridコンポーネントの読み込み
+ コンテナコンポーネントで使用するAEM Reactコアコンポーネント（テキストおよび画像）を読み込んで登録します。

### ResponsiveGridコンテナコンポーネントでの読み込み

編集可能な領域をホームビューに配置するには、次の操作を行う必要があります。

1. `@adobe/aem-react-editable-components`からResponsiveGridコンポーネントを読み込みます。
1. `withMappable`を使用して登録し、開発者がSPAに配置できるようにします。
1. また、他のコンテナコンポーネントで再利用できるように、`MapTo`に登録し、コンテナを効果的にネストします。

次の手順を実行します。

1. IDEでSPAプロジェクトを開きます
1. `src/components/aem/AEMResponsiveGrid.js`にReactコンポーネントを作成する
1. 次のコードを`AEMResponsiveGrid.js`に追加します。

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

このコードは、`AEMTitle.js`でAEM Reachコアコンポーネントのタイトルコンポーネント](./spa-fixed-component.md)を[読み込んだのと似ています。


`AEMResponsiveGrid.js`ファイルは次のようになります。

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### AEMResponsiveGrid SPAコンポーネントの使用

AEM ResponsiveGridコンポーネントがに登録され、SPA内で使用できるようになったので、ホームビューに配置できます。

1. `react-app/src/App.js`を開いて編集します
1. `AEMResponsiveGrid`コンポーネントを読み込み、`<AEMTitle ...>`コンポーネントの上に配置します。
1. `<AEMResponsiveGrid...>`コンポーネントに次の属性を設定します
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   これは、この`AEMResponsiveGrid`コンポーネントに対し、AEMリソースからコンテンツを取得するように指示します。

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`は、 `Remote SPA Page` AEMテンプレートで定義された`responsivegrid`ノードにマッピングされ、 `Remote SPA Page` AEMテンプレートから作成された新しいAEMページに自動的に作成されます。

   `App.js`を更新して、`<AEMResponsiveGrid...>`コンポーネントを追加します。

   ```
   ...
   import AEMResponsiveGrid from './components/aem/AEMResponsiveGrid';
   ...
   
   function Home() {
   return (
       <div className="Home">
           <AEMResponsiveGrid
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='root/responsivegrid'/>
   
           <AEMTitle
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='title'/>
           <Adventures />
       </div>
   );
   }
   ```

`Apps.js`ファイルは次のようになります。

![App.js](./assets/spa-container-component/app-js.png)

## 編集可能なコンポーネントの作成

柔軟なオーサリングエクスペリエンスコンテナの効果を最大限に引き出すには、SPA Editorを使用します。 編集可能なタイトルコンポーネントは既に作成されていますが、作成者が新しく追加されたコンテナコンポーネントでテキストおよび画像AEM WCMコアコンポーネントを使用できるように、さらに少し作成します。

### テキストコンポーネント

1. IDEでSPAプロジェクトを開きます
1. `src/components/aem/AEMText.js`にReactコンポーネントを作成する
1. 次のコードを`AEMText.js`に追加します。

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

`AEMText.js`ファイルは次のようになります。

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### 画像コンポーネント

1. IDEでSPAプロジェクトを開きます
1. `src/components/aem/AEMImage.js`にReactコンポーネントを作成する
1. 次のコードを`AEMImage.js`に追加します。

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. `AEMImage.scss`のカスタムスタイルを提供するSCSSファイル`src/components/aem/AEMImage.scss`を作成します。 これらのスタイルは、AEM ReactコアコンポーネントのBEM表記のCSSクラスをターゲットにしています。
1. 次のSCSSを`AEMImage.scss`に追加します。

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. `AEMImage.js`に`AEMImage.scss`を読み込む

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

`AEMImage.js`と`AEMImage.scss`は次のようになります。

![AEMImage.jsとAEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### 編集可能なコンポーネントの読み込み

新しく作成された`AEMText`と`AEMImage` SPAコンポーネントは、SPAで参照され、AEMから返されるJSONに基づいて動的にインスタンス化されます。 これらのコンポーネントをSPAで確実に使用できるようにするには、`App.js`にそれらのコンポーネントのimport文を作成します

1. IDEでSPAプロジェクトを開きます
1. ファイル`src/App.js`を開きます。
1. `AEMText`と`AEMImage`のimport文を追加します

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


結果は次のようになります。

![Apps.js](./assets/spa-container-component/app-js-imports.png)

これらのインポートが&#x200B;_追加_&#x200B;されない場合、`AEMText`および`AEMImage`コードはSPAによって呼び出されないので、指定されたリソースタイプに対してコンポーネントが登録されません。

## AEMでのコンテナの設定

AEMコンテナコンポーネントは、ポリシーを使用して、許可されるコンポーネントを指定します。 SPA Editorを使用する場合、SPAコンポーネントがマッピングされたAEM WCMコアコンポーネントのみがSPAでレンダリング可能なので、これは重要な設定です。 SPA実装を提供したコンポーネントのみが許可されていることを確認します。

+ `AEMTitle` マッピング済み  `wknd-app/components/title`
+ `AEMText` マッピング済み  `wknd-app/components/text`
+ `AEMImage` マッピング済み  `wknd-app/components/image`

リモートSPAページテンプレートのreponsivegridコンテナを構成するには：

1. AEMオーサーにログインします。
1. __ツール/一般/テンプレート/WKNDアプリ__&#x200B;に移動します。
1. __レポートのSPAページ__&#x200B;を編集します

   ![レスポンシブグリッドポリシー](./assets/spa-container-component/templates-remote-spa-page.png)

1. 右上のモード切り替えボタンで「__構造__」を選択します。
1. __レイアウトコンテナ__&#x200B;をタップして選択します。
1. ポップアップバーの&#x200B;__ポリシー__&#x200B;アイコンをタップします

   ![レスポンシブグリッドポリシー](./assets/spa-container-component/templates-policies-action.png)

1. 右側の「__許可されているコンポーネント__」タブで、「__WKND APP - CONTENT__」を展開します。
1. 次の項目のみが選択されていることを確認します。
   + 画像
   + テキスト
   + タイトル

   ![リモートSPAページ](./assets/spa-container-component/templates-allowed-components.png)

1. __完了__&#x200B;をタップします。

## AEMでのコンテナのオーサリング

SPAを更新して`<AEMResponsiveGrid...>`を埋め込み、3つのAEM React Coreコンポーネント(`AEMTitle`、`AEMText`、`AEMImage`)をラッパーし、AEMを適切なテンプレートポリシーで更新すると、コンテナコンポーネントでコンテンツのオーサリングを開始できます。

1. AEMオーサーにログインします。
1. __サイト/WKNDアプリ__&#x200B;に移動します。
1. 「__ホーム__」をタップし、上部のアクションバーから「__編集__」を選択します
   + 「Hello World」テキストコンポーネントが表示されます。これは、AEMプロジェクトのアーキタイプからプロジェクトを生成する際に、自動的に追加されたものです
1. ページエディターの右上にあるモードセレクターから「__編集__」を選択します。
1. タイトルの下にある&#x200B;__レイアウトコンテナ__&#x200B;編集可能領域を見つけます
1. __ページエディターのサイドバー__&#x200B;を開き、__コンポーネントビュー__&#x200B;を選択します。
1. 次のコンポーネントを&#x200B;__レイアウトコンテナ__&#x200B;にドラッグします。
   + 画像
   + タイトル
1. コンポーネントをドラッグして、次の順序に並べ替えます。
   1. タイトル
   1. 画像
   1. テキスト
1. ____ Authorthe  ____ Titlecomponent
   1. タイトルコンポーネントをタップし、__レンチ__&#x200B;アイコンをタップして、タイトルコンポーネントを&#x200B;__編集__&#x200B;します。
   1. 次のテキストを追加します。
      + タイトル：__夏が来るので、それを最大限に活かそう！__
      + 型：__H1__
   1. __完了__&#x200B;をタップします。
1. ____ Imagecomponentを作成 ____ する
   1. 画像コンポーネントの（アセットビューに切り替えた後の）サイドバーから画像をドラッグします
   1. 画像コンポーネントをタップし、__レンチ__&#x200B;アイコンをタップして編集します
   1. 「 __画像は装飾画像__ 」チェックボックスをオンにします。
   1. __完了__&#x200B;をタップします。
1. ____ Textcomponentを作成 ____ する
   1. テキストコンポーネントをタップし、__レンチ__&#x200B;アイコンをタップして、テキストコンポーネントを編集します。
   1. 次のテキストを追加します。
      + _今、1週間の冒険で15%、2週間以上の冒険で20%オフにできます。チェックアウト時に、キャンペーンコードSUMMERISCOMINGを追加して、割引を受け取ります。_
   1. __完了__&#x200B;をタップします。

1. コンポーネントがオーサリングされ、垂直にスタックされます。

   ![作成したコンポーネント](./assets/spa-container-component/authored-components.png)

   コンポーネントのサイズとレイアウトを調整するには、 AEMレイアウトモードを使用します。

1. 右上のモードセレクターを使用して、__レイアウトモード__&#x200B;に切り替えます。
1. ____ 画像コンポーネントとテキストコンポーネントを並べて表示するようにサイズ変更する
   + ____ Imagecomponentは8列幅にす __る必要があります__
   + ____ テキストコンポーネントは幅 __が3列である必要があります__

   ![レイアウトコンポーネント](./assets/spa-container-component/layout-components.png)

1. ____ AEMページエディターでの変更のプレビュー
1. [http://localhost:3000](http://localhost:3000)上でローカルで実行されているWKNDアプリを更新し、作成した変更を確認します。

   ![SPAのコンテナコンポーネント](./assets/spa-container-component/localhost-final.png)


## バリデーターが

作成者がWKNDアプリに編集可能なコンポーネントを追加できるコンテナコンポーネントを追加しました。 次の方法がわかりました。

+ SPAでAEM React編集可能コンポーネントのResponsiveGridコンポーネントを使用します。
+ コンテナコンポーネントを使用して、SPAで使用するAEM Reactコアコンポーネント（テキストおよび画像）を登録します。
+ リモートSPAページテンプレートの設定で、SPAが有効なコアコンポーネントを許可する
+ コンテナコンポーネントへの編集可能なコンポーネントの追加
+ SPA Editorでのオーサーコンポーネントとレイアウトコンポーネント

## 次の手順

次の手順では、同じ方法を使用して、SPAのアドベンチャー詳細ルート](./spa-dynamic-routes.md)に編集可能なコンポーネントを追加します。[
