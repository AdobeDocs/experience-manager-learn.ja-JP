---
title: 編集可能な React コンテナコンポーネントをリモートSPAに追加
description: AEM作成者がコンポーネントをリモートSPAにドラッグ&ドロップできるように、編集可能なコンテナコンポーネントをリモート作成者に追加する方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 12%

---

# 編集可能なコンテナコンポーネント

[固定コンポーネント](./spa-fixed-component.md) では、SPAコンテンツを柔軟にオーサリングできますが、このアプローチは堅牢で、開発者は編集可能コンテンツの正確な構成を定義する必要があります。 作成者が例外的なエクスペリエンスを作成できるように、SPA Editor では、SPAでのコンテナコンポーネントの使用をサポートしています。 コンテナコンポーネントを使用すると、作成者は、従来のAEM Sitesオーサリングと同様に、許可されたコンポーネントをコンテナにドラッグ&amp;ドロップしたり、オーサリングしたりできます。

![編集可能なコンテナコンポーネント](./assets/spa-container-component/intro.png)

この章では、編集可能なコンテナをホームビューに追加し、作成者が、SPA内で直接編集可能な React コンポーネントを使用して、リッチなコンテンツエクスペリエンスを作成およびレイアウトできるようにします。

## WKND アプリの更新

コンテナコンポーネントをホームビューに追加するには：

+ AEM React 編集可能コンポーネントの `ResponsiveGrid` コンポーネント
+ ResponsiveGrid コンポーネントで使用するカスタム編集可能 React コンポーネント（テキストおよび画像）を読み込んで登録します。

### ResponsiveGrid コンポーネントの使用

編集可能な領域をホームビューに追加するには：

1. `react-app/src/components/Home.js` を開いて編集する
1. 次をインポート： `ResponsiveGrid` コンポーネントから `@adobe/aem-react-editable-components` をクリックし、 `Home` コンポーネント。
1. 次の属性を `<ResponsiveGrid...>` コンポーネント
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   これは、`ResponsiveGrid` コンポーネントが AEM リソースからコンテンツを取得するように指示するものです。

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   The `itemPath` は、 `responsivegrid` で定義されたノード `Remote SPA Page` AEMテンプレートが作成され、 `Remote SPA Page` AEM Template.

   更新 `Home.js` を追加します。 `<ResponsiveGrid...>` コンポーネント。

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Home.js` ファイルは次のようになります。

![Home.js](./assets/spa-container-component/home-js.png)

## 編集可能なコンポーネントの作成

柔軟なオーサリングエクスペリエンスコンテナの効果を最大限に引き出すには、SPA Editor に用意されています。 編集可能なタイトルコンポーネントは既に作成されていますが、新しく追加された ResponsiveGrid コンポーネントで編集可能なテキストコンポーネントと画像コンポーネントを作成者が使用できるように、さらにいくつか作成します。

新しい編集可能なテキストおよび画像 React コンポーネントは、で書き出した編集可能なコンポーネント定義パターンを使用して作成されます。 [編集可能な固定コンポーネント](./spa-fixed-component.md).

### 編集可能なテキストコンポーネント

1. IDE で SPA プロジェクトを開きます。
1. `src/components/editable/core/Text.js` で React コンポーネントを作成
1. 次のコードをに追加します。 `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. 編集可能な React コンポーネントの作成先： `src/components/editable/EditableText.js`
1. 次のコードをに追加します。 `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

編集可能なテキストコンポーネントの実装は、次のようになります。

![編集可能なテキストコンポーネント](./assets/spa-container-component/text-js.png)

### 画像コンポーネント

1. IDE で SPA プロジェクトを開きます。
1. `src/components/editable/core/Image.js` で React コンポーネントを作成
1. 次のコードをに追加します。 `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. 編集可能な React コンポーネントの作成先： `src/components/editable/EditableImage.js`
1. 次のコードをに追加します。 `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. SCSS ファイルの作成 `src/components/editable/EditableImage.scss` には、 `EditableImage.scss`. これらのスタイルは、編集可能な React コンポーネントの CSS クラスをターゲットにしています。
1. 次の SCSS をに追加します。 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. インポート `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

編集可能な画像コンポーネントの実装は、次のようになります。

![編集可能な画像コンポーネント](./assets/spa-container-component/image-js.png)


### 編集可能なコンポーネントの読み込み

新しく作成された `EditableText` および `EditableImage` React コンポーネントはSPAで参照され、AEMから返される JSON に基づいて動的にインスタンス化されます。 これらのコンポーネントをSPAで確実に使用できるようにするには、次の場所にそれらのインポート文を作成します。 `Home.js`

1. IDE で SPA プロジェクトを開きます。
1. ファイル `src/Home.js` を開きます
1. 次のインポート文を追加： `AEMText` および `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

結果は次のようになります。

![Home.js](./assets/spa-container-component/home-js-imports.png)

これらのインポートが _not_ こう付け加えた。 `EditableText` および `EditableImage` コードはSPAによって呼び出されないので、コンポーネントは指定されたリソースタイプにマッピングされません。

## AEMでのコンテナの設定

AEMコンテナコンポーネントは、ポリシーを使用して、許可されるコンポーネントを指定します。 SPAエディターを使用する場合、SPAコンポーネントが対応する対応コンポーネントをマッピングしているAEMコンポーネントのみがSPAでレンダリング可能なので、これは重要な設定です。 に対してSPA実装を提供したコンポーネントのみが許可されていることを確認します。

+ `EditableTitle` マッピング先： `wknd-app/components/title`
+ `EditableText` マッピング先： `wknd-app/components/text`
+ `EditableImage` マッピング先： `wknd-app/components/image`

リモートSPAページテンプレートの reponsivegrid コンテナを構成するには、次の手順を実行します。

1. AEM オーサーにログインします。
1. に移動します。 __ツール/一般/テンプレート/WKND アプリ__
1. 編集 __レポートSPAページ__

   ![レスポンシブグリッドポリシー](./assets/spa-container-component/templates-remote-spa-page.png)

1. 選択 __構造__ 右上のモード切り替えボタン
1. タップして __レイアウトコンテナ__
1. 次をタップします。 __ポリシー__ ポップアップバーのアイコン

   ![レスポンシブグリッドポリシー](./assets/spa-container-component/templates-policies-action.png)

1. 右側の、 __許可されたコンポーネント__ タブ、展開 __WKND アプリ — コンテンツ__
1. 次の項目のみが選択されていることを確認します。
   + 画像
   + テキスト
   + タイトル

   ![リモートSPAページ](./assets/spa-container-component/templates-allowed-components.png)

1. 「__完了__」をタップします

## AEMでのコンテナのオーサリング

SPAが更新されて `<ResponsiveGrid...>`、編集可能な React コンポーネント (`EditableTitle`, `EditableText`、および `EditableImage`) が適用され、AEMが対応するテンプレートポリシーで更新されました。コンテナコンポーネントでコンテンツのオーサリングを開始できます。

1. AEM オーサーにログインします。
1. __サイト／WKND アプリ__&#x200B;に移動します。 
1. 「__ホーム__」をタップし、上部のアクションバーの「__編集__」を選択します。
   + 「Hello World」テキストコンポーネントが表示されます。これは、AEMプロジェクトアーキタイプからプロジェクトを生成する際に、自動的に追加されたものです
1. 選択 __編集__ ページエディターの右上にあるモードセレクターから
1. 次を見つけます。 __レイアウトコンテナ__ タイトルの下の編集可能な領域
1. __ページエディターのサイドバー__&#x200B;をクリックし、「__コンポーネント表示__」を選択します。
1. 次のコンポーネントを __レイアウトコンテナ__
   + 画像
   + タイトル
1. コンポーネントをドラッグして、次の順序に並べ替えます。
   1. タイトル
   1. 画像
   1. テキスト
1. __作成者__ の __タイトル__ コンポーネント
   1. タイトルコンポーネントをタップし、 __レンチ__ アイコン __編集__ タイトルコンポーネント
   1. 次のテキストを追加します。
      + タイトル： __夏が来た、それを最大限に活用しよう！__
      + タイプ： __H1__
   1. 「__完了__」をタップします
1. __作成者__ の __画像__ コンポーネント
   1. 画像コンポーネント上の（アセットビューに切り替えた後の）サイドバーから画像をドラッグします。
   1. 画像コンポーネントをタップし、 __レンチ__ 編集するアイコン
   1. 次を確認します。 __画像は装飾画像__ チェックボックス
   1. 「__完了__」をタップします
1. __作成者__ の __テキスト__ コンポーネント
   1. テキストコンポーネントをタップし、 __レンチ__ アイコン
   1. 次のテキストを追加します。
      + _今すぐ、1 週間の冒険で 15%、2 週間以上の冒険で 20%オフにできます！ チェックアウト時に、キャンペーンコード SUMMERISCOMING を追加して、割引を受け取ります。_
   1. 「__完了__」をタップします

1. コンポーネントがオーサリングされましたが、垂直にスタックします。

   ![作成済みコンポーネント](./assets/spa-container-component/authored-components.png)

AEMレイアウトモードを使用して、コンポーネントのサイズとレイアウトを調整できます。

1. 切り替え先 __レイアウトモード__ 右上の mode-selector の使用
1. __サイズ変更__ 画像コンポーネントとテキストコンポーネント（並べて表示されるように）
   + __画像__ コンポーネントは __8 列幅__
   + __テキスト__ コンポーネントは __3 列幅__

   ![レイアウトコンポーネント](./assets/spa-container-component/layout-components.png)

1. __AEM ページエディターで変更内容をプレビュー__
1. ローカルで実行している WKND アプリを更新 [http://localhost:3000](http://localhost:3000) 作成した変更を表示するには、以下を実行します。

   ![SPAのコンテナコンポーネント](./assets/spa-container-component/localhost-final.png)


## おめでとうございます。

作成者が WKND アプリに編集可能なコンポーネントを追加できるコンテナコンポーネントが追加されました。 次の方法を学習しました。

+ AEM React 編集可能コンポーネントの `ResponsiveGrid` SPAのコンポーネント
+ コンテナコンポーネントを使用して、SPAで使用する編集可能な React コンポーネント（テキストおよび画像）を作成および登録します。
+ SPAが有効なコンポーネントを許可するようにリモートSPAページテンプレートを設定します
+ コンテナコンポーネントへの編集可能なコンポーネントの追加
+ SPA Editor でのオーサリングおよびレイアウトコンポーネント

## 次の手順

次の手順では、同じ方法を使用して、 [Adventure Details ルートに編集可能なコンポーネントを追加する](./spa-dynamic-routes.md) をSPAに追加します。
