---
title: リモートSPAに追加対する編集可能な固定コンポーネント
description: 編集可能な固定コンポーネントをリモートSPAに追加する方法について説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター，コアコンポーネント， API，開発
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---


# 編集可能な固定コンポーネント

編集可能なリアクトコンポーネントは、「固定」にすることも、SPA表示にハードコードすることもできます。 これにより、開発者はSPAエディタ互換のコンポーネントをSPA表示に配置し、AEM editorでコンポーネントのコンテンツを作成できます。

![固定コンポーネント](./assets/spa-fixed-component/intro.png)

この章では、`Home.js`内のハードコードされたテキストであるホーム表示のタイトル「Current Adventures」を、固定された編集可能なタイトルコンポーネントに置き換えます。 固定コンポーネントは、タイトルの配置を保証しますが、タイトルのテキストを作成したり、開発サイクル以外で変更したりすることもできます。

## WKNDアプリの更新

固定コンポーネントをホーム表示に追加するには：

+ AEM React Core Component Titleコンポーネントを読み込み、プロジェクトのタイトルのリソースタイプに登録します
+ 編集可能なタイトルコンポーネントをSPAホーム表示に配置する

### AEM React Core ComponentのTitleコンポーネントでの読み込み

SPAホーム表示で、ハードコードされたテキスト`<h2>Current Adventures</h2>`をAEM React Core Components&#39; Titleコンポーネントに置き換えます。 Titleコンポーネントを使用する前に、次の作業を行う必要があります。

1. `@adobe/aem-core-components-react-base`からTitleコンポーネントをインポート
1. `withMappable`を使って登録し、開発者がSPAに置けるようにする
1. また、`MapTo`に登録して、[コンテナコンポーネントの後の](./spa-container-component.md)で使用できるようにします。

次の手順を実行します。

1. IDEの`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`にあるリモートSPAプロジェクトを開きます。
1. `react-app/src/components/aem/AEMTitle.js`にReactコンポーネントを作成
1. 追加次のコードを`AEMTitle.js`に送信します。

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

導入の詳細については、コードのコメントを読んでください。

`AEMTitle.js`ファイルは次のようになります。

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### AEMTitleコンポーネントの使用

AEM React Core ComponentのTitleコンポーネントがReactアプリに登録され、Reactアプリ内で使用できるようになったので、ホーム表示のハードコードされたタイトルテキストを置き換えます。

1. 編集 `react-app/src/App.js`
1. 下の`Home()`で、ハードコードされたタイトルを新しい`AEMTitle`コンポーネントに置き換えます。

   ```
   <h2>Current Adventures</h2>
   ```

   を次のタグに置換します。

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   `Apps.js`を次のコードで更新します。

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Apps.js`ファイルは次のようになります。

![App.js](./assets/spa-fixed-component/app-js.png)

## AEMでのTitleコンポーネントの作成

1. AEM作成者にログインする
1. __サイト/WKNDアプリ__&#x200B;に移動します。
1. 「__ホーム__」をタップし、上部のアクションバーで「__編集__」を選択します
1. ページエディターの右上にある編集モードセレクターで、「__編集__」を選択します。
1. WKNDロゴの下のデフォルトのタイトルテキストの上、およびアドベンチャーリストの上にマウスポインターを置くと、青い編集のアウトラインが表示されます
1. をタップしてコンポーネントのアクションバーを表示し、__レンチ__&#x200B;をタップして編集します

   ![タイトルコンポーネントのアクションバー](./assets/spa-fixed-component/title-action-bar.png)

1. タイトルコンポーネントのオーサリング：
   + タイトル：__WKNDの冒険__
   + 種類/サイズ：__H2__

      ![タイトルコンポーネントダイアログ](./assets/spa-fixed-component/title-dialog.png)

1. 「__完了__」をタップして保存します
1. AEM SPAエディタでの変更のプレビュー
1. [http://localhost:3000](http://localhost:3000)上でローカルで実行しているWKNDアプリを更新し、作成したタイトルの変更がすぐに反映されることを確認します。

   ![SPAのタイトルコンポーネント](./assets/spa-fixed-component/title-final.png)

## バリデーターが

WKNDアプリに、固定で編集可能なコンポーネントが追加されました。 これで、次の方法がわかりました。

+ SPAでAEM React Coreコンポーネントの読み込みと再利用
+ SPA追加の固定された、しかし編集可能なコンポーネント
+ AEMでの固定コンポーネントのオーサリング
+ 作成したコンテンツをリモートSPAで確認する

## 次の手順

次の手順は、AEM ResponsiveGridコンテナコンポーネント](./spa-container-component.md)をSPAに[追加し、作成者がSPAにコンポーネントを追加して編集できるようにします。
