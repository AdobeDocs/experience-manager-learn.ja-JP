---
title: AEMヘッドレス API と React - AEMヘッドレスの最初のチュートリアル
description: AEM GraphQL API からコンテンツフラグメントデータを取得し、React アプリで表示する方法を説明します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEMヘッドレス API と React

このチュートリアルの章では、AEMヘッドレス SDK を使用してAdobe Experience Manager(AEM) ヘッドレス API と接続するための React アプリの設定について説明します。 AEM GraphQL API からコンテンツフラグメントデータを取得し、React アプリで表示する方法について説明します。

AEMヘッドレス API を使用すると、任意のクライアントアプリからAEMコンテンツにアクセスできます。 AEMヘッドレス SDK を使用してAEMヘッドレス API に接続するように React アプリを設定する手順を説明します。 この設定により、React アプリとAEMの間に再利用可能な通信チャネルが確立されます。

次に、AEMヘッドレス SDK を使用して、AEM GraphQL API からコンテンツフラグメントデータを取得します。 AEMのコンテンツフラグメントは、構造化されたコンテンツ管理を提供します。 AEMヘッドレス SDK を使用すると、GraphQLを使用してコンテンツフラグメントデータのクエリや取得を簡単におこなえます。

コンテンツフラグメントデータを取得したら、それを React アプリに統合します。 データを魅力的な方法で書式設定し表示する方法を学びます。 React コンポーネントでのコンテンツフラグメントデータの処理とレンダリングのベストプラクティスを取り上げ、アプリの UI とシームレスに統合します。

チュートリアル全体で、説明、コード例、実用的なヒントを提供します。 最終的に、React アプリを設定してAEMヘッドレス API に接続し、AEMヘッドレス SDK を使用してコンテンツフラグメントデータを取得し、React アプリでシームレスに表示することができます。 それでは、始めましょう。


## React アプリのクローン

1. 次のアプリを複製： [Github](https://github.com/lamontacrook/headless-first/tree/main) 次のコマンドをコマンドラインで実行します。

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. を `headless-first` ディレクトリを開き、依存関係をインストールします。

   ```
   $ cd headless-first
   $ npm ci
   ```

## React アプリの設定

1. という名前のファイルを作成します。 `.env` プロジェクトのルートに配置します。 In `.env` 次の値を設定します。

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Cloud Manager で開発者トークンを取得できます。 にログインします。 [AdobeCloud Manager](https://experience.adobe.com/). クリック __Experience Manager/Cloud Manager__. 適切なプログラムを選択し、環境の横にある省略記号をクリックします。

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. をクリックします。 __統合__ タブ
   1. クリック __「ローカルトークン」タブと「ローカル開発トークンを取得」__ ボタン
   1. クローズしたクォーテーションの前まで、オープンクォーテーションの後のアクセストークンをコピーします。
   1. コピーしたトークンをの値として貼り付けます。 `REACT_APP_TOKEN` 内 `.env` ファイル。
   1. 次に、を実行してアプリを作成します。 `npm ci` コマンドライン上で
   1. 次に、React アプリを起動し、次を実行します。 `npm run start` コマンドライン上で
   1. In [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) という名前のファイル `context.js`  には、 `.env` ファイルをデスクトップアプリケーションのコンテキストに追加します。

## React アプリの実行

1. 次を実行して React アプリを起動します。 `npm run start` コマンドライン上で

   ```
   $ npm run start
   ```

   React アプリが起動し、次の操作を行うためのブラウザーウィンドウが開きます。 `http://localhost:3000`. React アプリに対する変更は、ブラウザーで自動的に再読み込みされます。

## AEMヘッドレス API への接続

1. React アプリをAEM as a Cloud Serviceに接続するには、次の点に注意してください。 `App.js`. 内 `React` インポート、追加 `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   インポート `AppContext` から `context.js` ファイル。

   ```javascript
   import { AppContext } from './utils/context';
   ```

   アプリコード内で、コンテキスト変数を定義します。

   ```javascript
   const context = useContext(AppContext);
   ```

   最後に、リターンコードを `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   参照用に、 `App.js` 今はこのようになるはずです。

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. 次をインポート： `AEMHeadless` SDK. この SDK は、アプリがAEMヘッドレス API とやり取りする際に使用するヘルパーライブラリです。

   この import 文を `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   以下を追加します。 `{ useContext, useEffect, useState }` から` React` インポート文。

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   次をインポート： `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   内部 `Home` コンポーネント、取得 `context` 変数を `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. 内でAEMヘッドレス SDK を初期化する  `useEffect()`AEMヘッドレス SDK は、  `context` 変数を変更します。

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > ここに `context.js` ～の下に立ち入る `/utils` は、 `.env` ファイル。 参照用に、 `context.url` は、AEMas a Cloud Service環境の URL です。 この `context.endpoint` は、前のレッスンで作成したエンドポイントへの完全パスです。 最後に、 `context.token` は開発者トークンです。


1. AEMヘッドレス SDK からのコンテンツを公開する React 状態を作成します。

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. アプリをAEMに接続します。 前のレッスンで作成した永続化されたクエリを使用します。 次のコードを `useEffect` AEMヘッドレス SDK の初期化後。 を `useEffect` ～に依存する  `context` 変数に関する情報を次に示します。


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. 開発者ツールのネットワーク表示を開き、 GraphQLリクエストを確認します。

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome Dev Tools](./assets/2/dev-tools.png)

   AEMヘッドレス SDK がGraphQLのリクエストをエンコードし、指定されたパラメーターを追加します。 ブラウザーでリクエストを開くことができます。

   >[!NOTE]
   >
   > リクエストはオーサー環境に送信されるので、同じブラウザーの別のタブで環境にログインする必要があります。


## コンテンツフラグメントコンテンツをレンダリング

1. アプリケーションでコンテンツフラグメントを表示します。 を返す `<div>` ティーザーのタイトルを入力します。

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   画面にティーザーのタイトルフィールドが表示されます。

1. 最後の手順では、ページにティーザーを追加します。 React ティーザーコンポーネントがパッケージに含まれています。 まず、インポートを含めます。 の上部 `home.js` ファイルに次の行を追加します。

   `import Teaser from '../../components/teaser/teaser';`

   return 文を更新します。

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   これで、フラグメント内にコンテンツが含まれたティーザーが表示されます。


## 次の手順

おめでとうございます。AEMヘッドレス SDK を使用して、AEMヘッドレス API と統合するよう React アプリを正常に更新しました。

次に、参照されるコンテンツフラグメントをAEMから動的にレンダリングする、より複雑な画像リストコンポーネントを作成します。

[次の章：画像リストコンポーネントの作成](./3-complex-components.md)