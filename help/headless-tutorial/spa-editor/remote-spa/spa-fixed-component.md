---
title: 編集可能な固定コンポーネントをリモートSPAに追加
description: 編集可能な固定コンポーネントをリモートSPAに追加する方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# 編集可能な固定コンポーネント

編集可能な React コンポーネントは、「固定」することも、SPAビューにハードコードすることもできます。 これにより、開発者はSPAエディター互換のコンポーネントをSPAビューに配置し、ユーザーはAEM SPAエディターでコンポーネントのコンテンツを作成できます。

![固定コンポーネント](./assets/spa-fixed-component/intro.png)

この章では、ホームビューのタイトル「Current Adventures」を置き換えます。これは、 `Home.js` 固定で編集可能なタイトルコンポーネントを使用 固定コンポーネントはタイトルの配置を保証しますが、タイトルのテキストをオーサリングし、開発サイクル外で変更することもできます。

## WKND アプリの更新

を追加するには、以下を実行します。 __固定__ コンポーネントをホームビューに追加します。

+ カスタムの編集可能なタイトルコンポーネントを作成し、プロジェクトのタイトルのリソースタイプに登録します。
+ 編集可能なタイトルコンポーネントをSPAホームビューに配置する

### 編集可能な React タイトルコンポーネントの作成

SPAホームビューで、ハードコードされたテキストを置き換えます `<h2>Current Adventures</h2>` カスタムの編集可能なタイトルコンポーネントを使用 タイトルコンポーネントを使用する前に、次の作業をおこなう必要があります。

1. カスタムタイトル React コンポーネントの作成
1. 次のメソッドを使用して、カスタムタイトルコンポーネントを修飾します `@adobe/aem-react-editable-components` 編集可能にする
1. 編集可能なタイトルコンポーネントの登録先 `MapTo` そのため、 [コンテナコンポーネントを後で](./spa-container-component.md).

次の手順を実行します。

1. 次の場所にあるリモートSPAプロジェクトを開く `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` IDE 内
1. React コンポーネントの作成先 `react-app/src/components/editable/core/Title.js`
1. 次のコードをに追加します。 `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   この React コンポーネントは、AEM SPA Editor を使用してまだ編集できないことに注意してください。 この基本コンポーネントは、次の手順で編集可能になります。

   実装の詳細については、コードのコメントを参照してください。

1. React コンポーネントの作成先 `react-app/src/components/editable/EditableTitle.js`
1. 次のコードをに追加します。 `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   この `EditableTitle` React コンポーネントは、 `Title` React コンポーネント。AEM SPA Editor で編集できるようにラッピングおよび修飾します。

### React EditableTitle コンポーネントの使用

EditableTitle React コンポーネントがに登録され、React アプリ内で使用できるようになったので、ホームビューのハードコードされたタイトルテキストを置き換えます。

1. 編集 `react-app/src/components/Home.js`
1. 内 `Home()` 下部に、 `EditableTitle` ハードコードされたタイトルを新しい `AEMTitle` コンポーネント：

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

この `Home.js` ファイルは次のようになります。

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEMでのタイトルコンポーネントのオーサリング

1. AEM オーサーにログインします。
1. に移動します。 __サイト/WKND アプリ__
1. タップ __ホーム__ を選択し、 __編集__ 上部のアクションバーから
1. 選択 __編集__ ページエディターの右上にある編集モードセレクターから、
1. WKND ロゴの下、およびアドベンチャーリストの上にあるデフォルトのタイトルテキストの上にマウスポインターを置き、青い編集のアウトラインが表示されるまでマウスを移動します。
1. をタップしてコンポーネントのアクションバーを表示し、 __レンチ__  編集

   ![タイトルコンポーネントのアクションバー](./assets/spa-fixed-component/title-action-bar.png)

1. タイトルコンポーネントをオーサリングします。
   + タイトル： __WKND アドベンチャ__
   + 種類/サイズ： __H2__

      ![タイトルコンポーネントダイアログ](./assets/spa-fixed-component/title-dialog.png)

1. タップ __完了__ 保存する
1. AEM SPA Editor で変更をプレビューします。
1. ローカルで実行している WKND アプリを更新 [http://localhost:3000](http://localhost:3000) 作成したタイトルの変更が直ちに反映されることを確認します。

   ![SPAのタイトルコンポーネント](./assets/spa-fixed-component/title-final.png)

## おめでとうございます。

WKND アプリに編集可能な固定コンポーネントが追加されました。 次の方法を理解できました。

+ SPAに対して固定（ただし編集可能）のコンポーネントを作成しました
+ AEMでの固定コンポーネントのオーサリング
+ 作成したコンテンツをリモートSPAで確認する

## 次の手順

次の手順は、次のとおりです。 [AEM ResponsiveGrid コンテナコンポーネントの追加](./spa-container-component.md) 作成者がSPAにコンポーネントを追加して編集できるSPAに
