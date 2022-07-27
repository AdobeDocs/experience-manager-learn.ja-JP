---
title: GraphQL API を使用してAEMに問い合わせる React アプリの構築 — AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の基本を学びます。 AEM GraphQL API からコンテンツ/データを取得する React アプリを作成し、AEMヘッドレス JS SDK の使用方法も確認してください。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: a073316f5541e392b5f5bd74c86ea146b1f80b9c
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 2%

---


# AEM GraphQL API を利用する React アプリの構築

この章では、AEM GraphQL API が外部アプリケーションでエクスペリエンスをどのように促すかについて説明します。

単純な React アプリを使用してクエリを実行し、表示します **チーム** および **人物** AEM GraphQL API で公開されるコンテンツ。 React の使用は、ほとんど重要ではありません。また、消費する外部アプリケーションは、あらゆるプラットフォームのあらゆるフレームワークに記述できます。

## 前提条件

このマルチパートチュートリアルの前の部分で説明した手順が完了していることを前提としています。 [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) がAEM as a Cloud Service Author サービスおよび Publish サービスにインストールされている。

_この章の IDE スクリーンショットは、次のものから取得します。 [Visual Studio Code](https://code.visualstudio.com/)_

次のソフトウェアをインストールする必要があります。

- [Node.js v12 以降](https://nodejs.org/ja/)
- [npm 6.4 以降](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Visual Studio Code](https://code.visualstudio.com/)

## 目的

以下の方法を学ぶ：

- サンプル React アプリをダウンロードして起動します。
- AEM GraphQL エンドポイントに対して、 [AEMヘッドレス JS SDK](https://github.com/adobe/aem-headless-client-js)
- AEMにチームのリストと参照メンバーを照会します
- AEMでチームメンバーの詳細をクエリ

## サンプル React アプリの取得

この章では、AEM GraphQL API とのやり取りに必要なコードを使用してスタブアウトサンプル React アプリを実装し、それらから取得したチームと人物のデータを表示します。

サンプルの React アプリのソースコードは、Github.com の次の場所で入手できます。

- https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial

React アプリを取得するには：

1. 次のサンプル WKND GraphQL React アプリをコピーします。 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone --branch git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. に移動します。 `basic-tutorial` フォルダーを開き、IDE で開きます。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode での React アプリ](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` をクリックして、AEM as a Cloud Service Publish サービスに接続します。

   - 設定 `REACT_APP_HOST_URI`AEMas a Cloud Serviceの公開 URL にするの値 ( 例： `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) および `REACT_APP_AUTH_METHOD`の値 `none`
   >[!NOTE]
   >
   > プロジェクト設定、コンテンツフラグメントモデル、コンテンツフラグメント、GraphQL エンドポイント、以前の手順で作成した永続的なクエリを公開していることを確認します。 ローカルのAEM SDK で上記の手順を実行した場合は、 `http://localhost:4502` および `REACT_APP_AUTH_METHOD`の値 `basic`.


1. コマンドラインから、 `aem-guides-wknd-graphql/basic-tutorial` フォルダー
1. React アプリを起動します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React アプリは次の場所で開発モードで開始します： [http://localhost:3000/](http://localhost:3000/). チュートリアル全体で React アプリに加えた変更は、自動的に反映されます。

## React アプリの詳細な構造

サンプル React アプリには、次の 3 つの主要な部分があります。

1. `src/api` には、AEMに対する GraphQL クエリを作成するために使用されるファイルが含まれます。
   - `src/api/aemHeadlessClient.js` AEMとの通信に使用するAEMヘッドレスクライアントを初期化およびエクスポートします
   - `src/api/usePersistedQueries.js` 実装 [カスタム React フック](https://reactjs.org/docs/hooks-custom.html) AEM GraphQL からにデータを返す `Teams.js` および `Person.js` コンポーネントを表示します。

1. `src/components/Teams.js` リストクエリを使用して、チームとそのメンバーのリストを表示します。
1. `src/components/Person.js` パラメータ化された単一結果クエリを使用して、1 人の人物の詳細を表示します。

## AEMHeadless クライアントの作成

レビュー `aemHeadlessClient.js` AEMとの通信に使用するAEMヘッドレスクライアントを初期化する方法について説明します。

1. `src/api/aemHeadlessClient.js`を開きます。
1. 行 1 ～ 40、インポート `AEMHeadless` SDK から、 `.env.development`、および `serviceUrl` 含まれる [開発プロキシ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) 設定。
1. 42～49 行は、AEMeadless クライアントをインスタンス化し、React アプリ全体で使用するために書き出すので、最も重要です。

   ```javascript
   // Initialize the AEM Headless Client and export it for other files to use
   const aemHeadlessClient = new AEMHeadless({
     serviceURL: serviceURL,
     endpoint: REACT_APP_GRAPHQL_ENDPOINT,
     auth: setAuthorization(),
   });
   
   export default aemHeadlessClient;
   ```

## AEM GraphQL エンドポイントへの接続

開く `usePersistedQueries.js` ジェネリックを実装する `fetchPersistedQuery(..)` 関数を使用してAEM GraphQL エンドポイントに接続します。 `fetchPersistedQuery(..)` を呼び出します。 `aemHeadlessClient` 書き出し元 `aemHeadlessClient.js`.

後で、カスタム React useEffect フックがこの関数を呼び出して、AEMから特定のデータを取得します。

1. In `src/api/usePersistedQueries.js` 更新 `fetchPersistedQuery(..)` を次のコードに置き換えます。

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## チーム機能の実装

次に、React アプリのメインビューにチームとそのメンバーを表示する機能を構築します。 この機能には、以下が必要です。

- 新しい [カスタム React useEffect フック](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` が `my-project/all-teams` クエリを保持し、AEMでチームコンテンツフラグメントのリストを返しました。
- React コンポーネント ( ) `src/components/Teams.js` 新しいカスタム React useEffect フックを呼び出し、チームデータをレンダリングします。

完了すると、アプリのメインビューにAEMのチームデータが入力されます。

![チーム表示](./assets/graphql-and-external-app/react-app__teams-view.png)

### 手順

1. `src/api/usePersistedQueries.js`を開きます。
1. 関数の場所 `useAllTeams()`
1. を作成するには、以下を実行します。 `useEffect` 持続クエリを呼び出すフック `my-project/all-teams` 経由 `fetchPersistedQuery(..)`に設定し、次のコードを追加します。 また、関連するデータは、AEM GraphQL 応答から、 `data?.teamList?.items`を使用すると、React ビューのコンポーネントを親 JSON 構造に依存しなくて済みます。

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. `src/components/Teams.js` を開きます。
1. 内 `Teams` React コンポーネント。を使用してAEMからチームのリストを取得します。 `useAllTeams()` フック

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```

1. ビューベースのデータ検証を実行し、返されたデータに基づいてエラーメッセージを表示するか、インジケーターを読み込みます。

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. 最後に、チームデータをレンダリングします。 GraphQL クエリから返された各チームは、指定された `Team` React のサブコンポーネント。

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## 担当者機能の実装

を使用 [チーム機能](#implement-teams-functionality) 完了、チームメンバーまたはユーザーの詳細の表示を処理する機能を実装します。

この機能には、以下が必要です。

- 新しい [カスタム React useEffect フック](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` パラメータ化を呼び出す `my-project/person-by-name` 永続的なクエリを返し、単一の個人レコードを返します。

- React コンポーネント ( ) `src/components/Person.js` クエリパラメーターとして人物のフルネームを使用し、新しいカスタム React useEffect フックを呼び出して、人物データをレンダリングします。

完了したら、Teams ビューで人物の名前を選択すると、人物ビューがレンダリングされます。

![Person](./assets/graphql-and-external-app/react-app__person-view.png)

1. `src/api/usePersistedQueries.js`を開きます。
1. 関数の場所 `usePersonByName(fullName)`
1. を作成するには、以下を実行します。 `useEffect` 持続クエリを呼び出すフック `my-project/all-teams` 経由 `fetchPersistedQuery(..)`に設定し、次のコードを追加します。 また、関連するデータは、AEM GraphQL 応答から、 `data?.teamList?.items`を使用すると、React ビューのコンポーネントを親 JSON 構造に依存しなくて済みます。

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. `src/components/Person.js` を開きます。
1. 内 `Person` React コンポーネント、解析 `fullName` ルートパラメーターを使用して、AEMから人物データを取得します。 `usePersonByName(fullName)` フック

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. ビューベースのデータ検証を実行し、返されたデータに基づいてエラーメッセージを表示するか、インジケーターを読み込みます。

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. 最後に、人物データをレンダリングします。

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## アプリを試す

アプリを確認する [http://localhost:3000/](http://localhost:3000/) をクリックし、 _メンバー_ リンク。 さらに、チームアルファにチームやメンバーを追加することもできます。

## フードの下で

ブラウザーの **開発者ツール** > **ネットワーク** および *フィルター* 対象 `all-teams` リクエストには、GraphQL API リクエストが表示されます。 `/graphql/execute.json/my-project/all-teams` ～に対して行われた `http://localhost:3000` および **NOT** ～の価値に反して `REACT_APP_HOST_URI`( 例： https://publish-p123-e456.adobeaemcloud.com)、これは、 [プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware` モジュール。


![プロキシを介した GraphQL API リクエスト](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


メイン `../setupProxy.js` ファイルと内部 `../proxy/setupProxy.auth.**.js` ファイルは、 `/content` および `/graphql` パスはプロキシ化され、静的アセットではないことを示します。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

ただし、これは実稼動のデプロイメントに適したオプションではありません。詳しくは、 _実稼動のデプロイメント_ 」セクションに入力します。

## おめでとうございます。{#congratulations}

おめでとうございます。AEM GraphQL API のデータを使用および表示する React アプリを基本的なチュートリアルの一部として正常に作成できました。
