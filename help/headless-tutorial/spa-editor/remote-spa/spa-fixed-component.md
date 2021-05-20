---
title: 編集可能な固定コンポーネントをリモートSPAに追加
description: 編集可能な固定コンポーネントをリモートSPAに追加する方法を説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---


# 編集可能な固定コンポーネント

編集可能なReactコンポーネントは、「固定」することも、SPAビューにハードコードすることもできます。 これにより、開発者は、SPAエディター互換のコンポーネントをSPAビューに配置し、ユーザーはAEM SPA Editorでコンポーネントのコンテンツを作成できます。

![固定コンポーネント](./assets/spa-fixed-component/intro.png)

この章では、`Home.js`内のハードコードされたテキストである、ホームビューのタイトル「Current Adventures」を、固定で編集可能なタイトルコンポーネントに置き換えます。 固定コンポーネントは、タイトルの配置を保証しますが、タイトルのテキストをオーサリングし、開発サイクル外で変更することもできます。

## WKNDアプリの更新

固定コンポーネントをホームビューに追加するには：

+ AEM Reactコアコンポーネントのタイトルコンポーネントを読み込み、プロジェクトのタイトルのリソースタイプに登録します。
+ 編集可能なタイトルコンポーネントをSPAホームビューに配置する

### AEM Reactコアコンポーネントのタイトルコンポーネントでの読み込み

SPA Homeビューで、ハードコードされたテキスト`<h2>Current Adventures</h2>`をAEM React Core Componentsのタイトルコンポーネントに置き換えます。 タイトルコンポーネントを使用する前に、次の作業を行う必要があります。

1. `@adobe/aem-core-components-react-base`からタイトルコンポーネントをインポートします。
1. `withMappable`を使用して登録し、開発者がSPAに配置できるようにします。
1. また、`MapTo`に登録して、後で[コンテナコンポーネント](./spa-container-component.md)で使用できるようにします。

次の手順を実行します。

1. IDEの`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`にあるリモートSPAプロジェクトを開きます。
1. `react-app/src/components/aem/AEMTitle.js`にReactコンポーネントを作成する
1. `AEMTitle.js`に次のコードを追加します。

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

実装の詳細については、コードのコメントを参照してください。

`AEMTitle.js`ファイルは次のようになります。

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### React AEMTitleコンポーネントの使用

AEM Reactコアコンポーネントのタイトルコンポーネントがに登録され、Reactアプリ内で使用できるようになったので、ホームビューのハードコードされたタイトルテキストを置き換えます。

1. 編集 `react-app/src/App.js`
1. 下部の`Home()`で、ハードコードされたタイトルを新しい`AEMTitle`コンポーネントに置き換えます。

   ```
   <h2>Current Adventures</h2>
   ```

   以下に置き換えます。

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

## AEMでのタイトルコンポーネントのオーサリング

1. AEMオーサーにログインします。
1. __サイト/WKNDアプリ__&#x200B;に移動します。
1. 「__ホーム__」をタップし、上部のアクションバーから「__編集__」を選択します
1. ページエディターの右上にある編集モードセレクターで「__編集__」を選択します。
1. WKNDロゴの下のデフォルトのタイトルテキストの上、およびアドベンチャーリストの上にマウスポインターを置き、青い編集の概要が表示されるまで表示されます
1. をタップしてコンポーネントのアクションバーを表示し、__レンチ__&#x200B;をタップして編集します。

   ![タイトルコンポーネントのアクションバー](./assets/spa-fixed-component/title-action-bar.png)

1. タイトルコンポーネントのオーサリング：
   + タイトル：__WKND冒険__
   + 種類/サイズ：__H2__

      ![タイトルコンポーネントダイアログ](./assets/spa-fixed-component/title-dialog.png)

1. __完了__&#x200B;をタップして保存します。
1. AEM SPA Editorでの変更のプレビュー
1. [http://localhost:3000](http://localhost:3000)上でローカルで実行されているWKNDアプリを更新し、作成したタイトルの変更がすぐに反映されることを確認します。

   ![SPAのタイトルコンポーネント](./assets/spa-fixed-component/title-final.png)

## バリデーターが

WKNDアプリに編集可能な固定コンポーネントが追加されました。 次の方法がわかりました。

+ SPAでのAEM Reactコアコンポーネントの読み込みと再利用
+ 固定で編集可能なコンポーネントをSPAに追加する
+ AEMでの固定コンポーネントのオーサリング
+ 作成したコンテンツをリモートSPAで参照する

## 次の手順

次の手順は、作成者がSPAにコンポーネントを追加し、編集可能なコンポーネントをSPAに追加できるように、AEM ResponsiveGridコンテナコンポーネント](./spa-container-component.md)を追加することです。[
