---
title: 編集可能な固定コンポーネントをリモート SPA に追加
description: 編集可能な固定コンポーネントをリモート SPA に追加する方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 164
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# 編集可能な固定コンポーネント

編集可能な React コンポーネントは、「固定」することも、SPA の表示にハードコードすることもできます。 これにより、デベロッパーは SPA エディター互換のコンポーネントを SPA 表示に配置し、ユーザーは AEM SPA エディターでコンポーネントのコンテンツを作成できるようになります。

![固定コンポーネント](./assets/spa-fixed-component/intro.png)

この章では、ホームビューのタイトル「Current Adventures」を置き換えます。これは、編集可能な固定タイトルコンポーネントを使用して `Home.js` でハードコード化されたテキストです。固定コンポーネントはタイトルの配置を保証しますが、タイトルのテキストの作成と開発サイクル外での変更は可能です。

## WKND アプリの更新

次のように、__固定__&#x200B;コンポーネントをホームビューに追加します。

+ カスタムの編集可能なタイトルコンポーネントを作成し、プロジェクトのタイトルのリソースタイプに登録します。
+ 編集可能なタイトルコンポーネントを SPA ホームビューに配置する

### 編集可能な React タイトルコンポーネントの作成

SPA ホームビューで、ハードコードされたテキスト `<h2>Current Adventures</h2>` をカスタムの編集可能なタイトルコンポーネントに置き換えます。タイトルコンポーネントを使用する前に、次の作業を行う必要があります。

1. カスタムタイトル React コンポーネントの作成
1. 編集可能にする `@adobe/aem-react-editable-components` のメソッドを使用し、カスタムタイトルコンポーネントをデコレートします。
1. `MapTo` で編集可能なタイトルコンポーネントを登録して、 [後からコンテナコンポーネント](./spa-container-component.md)で使用できるようにします。

次の手順を実行します。

1. IDE の `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` でリモート SPA プロジェクトを開く
1. `react-app/src/components/editable/core/Title.js` で React コンポーネントを作成
1. `Title.js` に次のコードを追加します。

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

   この React コンポーネントは、まだ AEM SPA エディターを使用して編集できないことに注意してください。 この基本コンポーネントは、次の手順で編集可能になります。

   実装の詳細については、コードのコメントを参照してください。

1. `react-app/src/components/editable/EditableTitle.js` で React コンポーネントを作成します 
1. `EditableTitle.js` に次のコードを追加します。

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

   この `EditableTitle` React コンポーネントは、 `Title` React コンポーネントをラップし、AEM SPA エディターで編集できるようにラップおよびデコレートします。

### React 編集可能タイルコンポーネントの使用

編集可能タイル React コンポーネントがReact アプリに登録され、React アプリ内で使用できるようになったので、ホームビューのハードコードされたタイトルテキストを置き換えます。

1. `react-app/src/components/Home.js` の編集
1. 下部の `Home()` で `EditableTitle` を読み込み、ハードコードされたタイトルを新しい `AEMTitle` コンポーネント：

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

`Home.js` ファイルは次のようになります。

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEM でのタイトルコンポーネントのオーサリング

1. AEM オーサーにログインします。
1. __サイト／WKND アプリ__&#x200B;に移動します。 
1. 「__ホーム__」をタップし、上部のアクションバーの「__編集__」を選択します。
1.  ページエディターの右上にある編集モードセレクターから「__編集__」を選択します。
1. 青い編集のアウトラインが表示されるまで、WKND ロゴの下、アドベンチャーリストの上にあるデフォルトのタイトルテキストの上にマウスポインターを置きます。
1. コンポーネントのアクションバーをタップして表示し、__レンチ__&#x200B;をタップして 編集します。

   ![タイトルコンポーネントのアクションバー](./assets/spa-fixed-component/title-action-bar.png)

1. タイトルコンポーネントを作成します。
   + タイトル： __WKND アドベンチャー__
   + 種類／サイズ：__H2__

     ![タイトルコンポーネントダイアログ](./assets/spa-fixed-component/title-dialog.png)

1. 「__完了__」をタップして保存します。
1. AEM SPA エディターで変更をプレビューします。
1. [http://localhost:3000](http://localhost:3000) でローカルに実行している WKND アプリを更新し、作成したタイトルの変更が直ちに反映されることを確認します。

   ![SPA のタイトルコンポーネント](./assets/spa-fixed-component/title-final.png)

## おめでとうございます。

WKND アプリに編集可能な固定コンポーネントが追加されました。 次の方法を学習しました。

+ SPA への固定（ただし編集可能）コンポーネントの作成
+ AEM での固定コンポーネントの作成
+ 作成したコンテンツのリモート SPA での確認

## 次の手順

次は、SPA に作成者が編集可能なコンポーネントを追加できるようにする [AEM ResponsiveGrid コンテナコンポーネントを SPA に追加](./spa-container-component.md)します。
