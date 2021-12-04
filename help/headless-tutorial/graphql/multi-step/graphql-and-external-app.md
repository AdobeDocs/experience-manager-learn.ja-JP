---
title: 外部アプリから GraphQL を使用したクエリAEM - AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の基本を学びます。 AEM GraphQL API を参照し、サンプルの WKND GraphQL React アプリを参照してください。 この外部アプリがAEMに対して GraphQL 呼び出しをおこない、エクスペリエンスを強化する方法を説明します。 基本的なエラー処理を実行する方法を説明します。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1382'
ht-degree: 1%

---

# 外部アプリから GraphQL を使用したクエリAEM

この章では、外部アプリケーションでエクスペリエンスを駆動するためにAEM GraphQL API を使用する方法について説明します。

このチュートリアルでは、シンプルな React アプリを使用して、AEM GraphQL API によって公開されたアドベンチャーコンテンツをクエリし、表示します。 React の使用は、ほとんど重要ではありません。また、消費する外部アプリケーションは、あらゆるプラットフォームのあらゆるフレームワークに記述できます。

## 前提条件

これは複数のパートから成るチュートリアルで、前のパートで説明した手順が完了していることを前提としています。

_この章の IDE スクリーンショットは、次のものから取得します。 [Visual Studio Code](https://code.visualstudio.com/)_

オプションで、のようなブラウザー拡張機能をインストールします。 [GraphQL ネットワークインスペクター](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) を使用して、GraphQL クエリの詳細を表示できます。

## 目的

この章では、次の方法について説明します。

* サンプル React アプリの機能を開始および理解する
* 外部アプリからAEM GraphQL エンドポイントへの呼び出しの実行方法を調べる
* GraphQL クエリを定義し、アクティビティ別に Adventures コンテンツフラグメントのリストをフィルタリングします
* React アプリを更新して、GraphQL（アクティビティ別の冒険のリスト）を介してフィルタリングするコントロールを提供します

## React アプリを起動します。

この章では、GraphQL 上でコンテンツフラグメントを使用するクライアントの開発に焦点を当てているので、サンプル [WKND GraphQL React アプリのソースコードをダウンロードし、設定する必要があります](../quick-setup/local-sdk.md) ローカルマシン上にある

React アプリの起動の概要について詳しくは、 [クイックセットアップ](../quick-setup/local-sdk.md) 章ただし、簡約命令は、次の通りとすることができる。

1. まだの場合は、次のサンプル WKND GraphQL React アプリをコピーしてください。 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. IDE で WKND GraphQL React アプリを開きます

   ![VSCode での React アプリ](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. コマンドラインから、 `react-app` フォルダー
1. WKND GraphQL React アプリを起動します。そのためには、プロジェクトルート ( `react-app` フォルダー )

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 次の場所でアプリを確認します。 [http://localhost:3000/](http://localhost:3000/). サンプル React アプリには、次の 2 つの主要な部分があります。

   * ホームエクスペリエンスは、WKND Adventures のインデックスとして機能します。 __冒険__ GraphQL を使用したAEMのコンテンツフラグメント。 この章では、このビューを変更して、アクティビティによる冒険のフィルタリングをサポートします。

      ![WKND GraphQL React アプリ — ホームエクスペリエンス](./assets/graphql-and-external-app/react-home-view.png)

   * アドベンチャーの詳細エクスペリエンスでは、GraphQL を使用して特定の __冒険__ コンテンツフラグメントを作成し、さらに多くのデータポイントを表示します。

      ![WKND GraphQL React アプリ — 詳細エクスペリエンス](./assets/graphql-and-external-app/react-details-view.png)

1. ブラウザーの開発ツールと、 [GraphQL ネットワークインスペクター](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) を使用して、AEMに送信された GraphQL クエリと、その JSON 応答を調べます。 この方法は、GraphQL のリクエストと応答を監視し、正しく定式化され、応答が期待どおりに行われていることを確認するために使用できます。

   ![adventureList の生のクエリ](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *React アプリからAEMに送信された GraphQL クエリ*

   ![GraphQL JSON の応答](assets/graphql-and-external-app/graphql-json-response.png)

   *AEMから React アプリへの JSON 応答*

   クエリと応答は、GraphiQL IDE で確認された内容と一致する必要があります。

   >[!NOTE]
   >
   > 開発時に、React アプリは、webpack 開発サーバーを通じてAEMに HTTP リクエストをプロキシするように設定されます。 React アプリがにリクエストを送信しています  `http://localhost:3000` は、で実行されている AEM オーサーサービスにプロキシします。 `http://localhost:4502`. ファイルを確認します。 `src/setupProxy.js` および `env.development` 」を参照してください。
   >
   > 非開発シナリオでは、React アプリがAEMに直接リクエストをおこなうように設定されます。

## アプリの GraphQL コードの参照

1. IDE でファイルを開きます。 `src/api/useGraphQL.js`.

   これは、 [React エフェクトフック](https://reactjs.org/docs/hooks-overview.html#effect-hook) をリッスンしてアプリの `query`、およびが変更されると、AEM GraphQL エンドポイントに対して HTTPPOSTリクエストをおこない、アプリに JSON 応答を返します。

   React アプリは、GraphQL クエリを作成する必要が生じるたびに、このカスタムを呼び出します `useGraphQL(query)` フックを渡し、AEMに送信する GraphQL を渡します。

   このフックは、 `fetch` HTTPPOSTGraphQL 要求を行うモジュール ( [Apollo GraphQL クライアント](https://www.apollographql.com/docs/react/) 同様に使用できます。

1. 開く `src/components/Adventures.js` IDE（ホームビューの冒険リストを担当）で、 `useGraphQL` フック

   このコードは、デフォルトの `query` を `allAdventuresQuery` このファイルの下で定義されたとおりです。

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   いつでも `query` 変数の変更 `useGraphQL` フックが呼び出され、AEMに対して GraphQL クエリが実行され、JSON が `data` 変数を使用して、アドベンチャーのリストをレンダリングします。

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   この `allAdventuresQuery` は、ファイルで定義されている定数の GraphQL クエリで、すべての Adventure コンテンツフラグメントに対してフィルタリングを行わずにクエリを実行し、ホームビューをレンダリングする必要のあるデータポイントのみを返します。

   ```javascript
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
               mimeType
               width
               height
             }
           }
         }
     }
   }
   `;
   ```

1. 開く `src/components/AdventureDetail.js`：アドベンチャーの詳細エクスペリエンスを表示する React コンポーネント。 この表示は、特定のコンテンツフラグメントを要求し、その JCR パスを一意の ID として使用して、提供された詳細をレンダリングします。

   同様に `Adventures.js`、カスタム `useGraphQL` React Hook は、AEMに対して GraphQL クエリを実行するために再利用されます。

   コンテンツフラグメントのパスは、コンポーネントの `props` top は、クエリするコンテンツフラグメントを指定するために使用します。

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   GraphQL パラメーター化クエリは、 `adventureDetailQuery(..)` 関数に渡され、 `useGraphQL(query)` AEMに対して GraphQL クエリを実行し、結果を `data` 変数を使用します。

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   この `adventureDetailQuery(..)` 関数は、AEMを使用するフィルタリング GraphQL クエリをラップするだけです `<modelName>ByPath` 構文を使用して、JCR パスで識別される単一のコンテンツフラグメントをクエリし、アドベンチャーの詳細をレンダリングするために必要な指定されたすべてのデータポイントを返します。

   ```javascript
   function adventureDetailQuery(_path) {
   return `{
       adventureByPath (_path: "${_path}") {
         item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
         }
       }
   }
   `;
   }
   ```

## パラメーター化された GraphQL クエリの作成

次に、React アプリを変更して、パラメーター化され、アドベンチャーのアクティビティによってホームビューを制限する GraphQL クエリをフィルタリングして実行します。

1. IDE で、次のファイルを開きます。 `src/components/Adventures.js`. このファイルは、Adventures カードをクエリして表示するホームエクスペリエンスの冒険コンポーネントを表します。
1. Inspect関数 `filterQuery(activity)`：未使用ですが、で冒険をフィルタリングする GraphQL クエリを作成する準備が整っています。 `activity`.

   このパラメーターに注意してください。 `activity` が、 `filter` の `adventureActivity` フィールドに値を入力する必要があります。

   ```javascript
   function filterQuery(activity) {
       return `
           {
           adventures (filter: {
               adventureActivity: {
               _expressions: [
                   {
                   value: "${activity}"
                   }
                 ]
               }
           }){
               items {
               _path
               adventureTitle
               adventurePrice
               adventureTripLength
               adventurePrimaryImage {
               ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
               }
               }
             }
         }
       }
       `;
   }
   ```

1. React Adventures コンポーネントの `return` 新しいパラメーター化されたボタンを呼び出すボタンを追加するステートメント `filterQuery(activity)` リストに載せる冒険を提供する。

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. 変更を保存し、Web ブラウザーで React アプリをリロードします。 上部に 3 つの新しいボタンが表示され、それらをクリックすると、AEMに対して、一致するアクティビティを持つアドベンチャーコンテンツフラグメントに対するクエリが自動的に再実行されます。

   ![アクティビティでアドベンチャをフィルタリング](./assets/graphql-and-external-app/filter-by-activity.png)

1. アクティビティのフィルターボタンをさらに追加してみます。 `Rock Climbing`, `Cycling` および `Skiing`

## GraphQL エラーの処理

GraphQL は厳密に型指定されているので、クエリが無効な場合は、役立つエラーメッセージを返す可能性があります。 次に、誤ったクエリをシミュレートして、返されたエラーメッセージを確認します。

1. ファイルを再度開きます。 `src/api/useGraphQL.js`. Inspectの次のスニペットを使用して、エラー処理を確認します。

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   応答に `errors` オブジェクト。 この `errors` GraphQL クエリに問題がある場合（スキーマに基づく未定義のフィールドなど）、AEMからオブジェクトが送信されます。 次がない場合： `errors` オブジェクト `data` が設定されて返されます。

   この `window.fetch` を含む `.catch` ～に対する声明 *キャッチ* 無効な HTTP リクエストや、サーバーへの接続が確立できない場合など、一般的なエラー。

1. `src/components/Adventures.js` ファイルを開きます。
1. を変更します。 `allAdventuresQuery` 無効なプロパティを含めるには `adventurePetPolicy`:

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
               mimeType
               width
               height
           }
           }
         }
       }
   }
   `;
   ```

   我々は知っている `adventurePetPolicy` はアドベンチャーモデルに含まれていないので、エラーがトリガーされます。

1. 変更を保存し、ブラウザーに戻ります。 次のようなエラーメッセージが表示されます。

   ![無効なプロパティエラー](assets/graphql-and-external-app/invalidProperty.png)

   GraphQL API は、 `adventurePetPolicy` が `AdventureModel` を渡すと、適切なエラーメッセージが返されます。

1. ブラウザーの開発者ツールを使用してAEMからの応答をInspectし、 `errors` JSON オブジェクト：

   ![エラー JSON オブジェクト](assets/graphql-and-external-app/error-json-response.png)

   この `errors` オブジェクトは詳細で、形式が正しくないクエリの場所やエラーの分類に関する情報が含まれます。

1. 戻る `Adventures.js` クエリの変更を元に戻し、アプリを正しい状態に戻します。

## おめでとうございます。{#congratulations}

おめでとうございます。サンプル WKND GraphQL React アプリのコードを確認し、パラメーター化されたを使用して GraphQL クエリをフィルタリングし、アクティビティ別の冒険をリストするように更新しました。 また、基本的なエラー処理を確認する機会も得られました。

## 次の手順 {#next-steps}

次の章では、 [フラグメントリファレンスを使用した高度なデータモデリング](./fragment-references.md) フラグメント参照機能を使用して、2 つの異なるコンテンツフラグメント間の関係を作成する方法を学びます。 また、GraphQL クエリを変更して、参照モデルのフィールドを含める方法についても説明します。
