---
title: GraphQL API を使用して AEM にクエリを実行する React アプリの作成 - AEM ヘッドレスの基本を学ぶ - GraphQL
description: Adobe Experience Manager（AEM）および GraphQL の概要AEM GraphQL API からコンテンツやデータを取得する React アプリを作成し、AEM ヘッドレス JS SDK の使用方法も確認します。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 100%

---


# AEM GraphQL API を使用する React アプリの作成

この章では、AEM GraphQL API が外部アプリケーションでエクスペリエンスを促進する方法について説明します。

シンプルな React アプリを使用してクエリを実行し、AEM GraphQL API が開示した&#x200B;**チーム**&#x200B;と&#x200B;**人物**&#x200B;のコンテンツを表示します。React の使用はあまり重要ではなく、消費する外部アプリケーションは、任意のプラットフォームの任意のフレームワークで作成できます。

## 前提条件

この多段階チュートリアルの前の部分で説明した手順が完了しているか、[basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) が AEM as a Cloud Service のオーサーサービスとパブリッシュサービスにインストールされていることを前提としています。

_この章の IDE スクリーンショットは、[Visual Studio Code](https://code.visualstudio.com/)から取得しています。_

次のソフトウェアがインストールされている必要があります。

- [Node.js v18](https://nodejs.org/ja)
- [Visual Studio Code](https://code.visualstudio.com/)

## 目的

次の方法を学びます。

- サンプルの React アプリをダウンロードして起動する
- [AEM ヘッドレス JS SDK](https://github.com/adobe/aem-headless-client-js) を使用して AEM GraphQL のエンドポイントにクエリを実行する
- AEM にチームのリストと参照メンバーに対するクエリを実行する
- AEM にチームメンバーの詳細に対するクエリを実行する

## サンプル React アプリの取得

この章では、AEM の GraphQL API と対話し、そこから取得したチームや人のデータを表示するために必要なコードを使用して、スタブ化されたサンプル React アプリを実装します。

サンプル React アプリのソース コードは、Github.com の <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial> で入手できます。

React アプリを取得するには：

1. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql) からサンプルの WKND GraphQL React アプリを複製します。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `basic-tutorial` フォルダーに移動し、IDE で開きます。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode での React アプリ](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. `.env.development` を更新して、AEM as a Cloud Service のパブリッシュサービスに接続します。

   - `REACT_APP_HOST_URI` の値を AEM as a Cloud Service の公開 URL（例：`REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`）に設定し、`REACT_APP_AUTH_METHOD` の値を `none` に設定します。

   >[!NOTE]
   >
   > プロジェクト設定、コンテンツフラグメントモデル、作成したコンテンツフラグメント、GraphQL エンドポイントおよび前の手順からの永続クエリが公開されていることを確認します。
   >
   > ローカルの AEM Author SDK で上記の手順を実行した場合は、`http://localhost:4502` を指定し、`REACT_APP_AUTH_METHOD` の値を `basic` に指定できます。


1. コマンドラインから、`aem-guides-wknd-graphql/basic-tutorial` フォルダーに移動します。

1. React アプリを起動します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React アプリは、開発モードで [http://localhost:3000/](http://localhost:3000/) で起動します。チュートリアルで React アプリに加えた変更は、直ちに反映されます。

![部分的に実装された React アプリ](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   この React アプリは部分的に実装されています。このチュートリアルの手順に従って、実装を完了します。実装が必要な JavaScript ファイルには次のコメントが含まれています。これらのファイルのコードを、必ずこのチュートリアルで指定したコードで追加または更新してください。
>
>
> //*********************************
>
>  // TODO  これを実装するには、AEM ヘッドレスチュートリアルの手順に従います。
>
>  //*********************************
>

## React アプリの詳細な構造

サンプル React アプリは、3 つの主要な部分から成り立っています。

1. `src/api` フォルダーには、AEM に対する GraphQL クエリを実行するために使用するファイルが含まれています。
   - `src/api/aemHeadlessClient.js` は、AEM との通信に使用する AEM ヘッドレスクライアントを初期化して書き出します。
   - `src/api/usePersistedQueries.js` は、[カスタム React フック](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components)を実装し、AEM GraphQL から `Teams.js` と `Person.js` のビューコンポーネントにデータを返します。

1. `src/components/Teams.js` ファイルは、リストクエリを使用して、チームとそのメンバーのリストを表示します。
1. `src/components/Person.js` ファイルは、パラメーター化された単一結果クエリを使用して、1 人の人物の詳細を表示します。

## AEMHeadless オブジェクトのレビュー

AEM との通信に使用する `AEMHeadless` オブジェクトの作成方法については、`aemHeadlessClient.js` ファイルを確認してください。

1. `src/api/aemHeadlessClient.js` を開きます。

1. 1 から 40 行目をレビューします。

   - [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js) からのインポート `AEMHeadless` 宣言、11 行目。

   - `.env.development` で定義された変数に基づく認証の設定、14 から 22 行目、アロー関数式`setAuthorization`、31 から 40 行目。

   - インクルードされた[開発プロキシ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests)設定の `serviceUrl` 設定、27行目。

1. 42 から 49 行目は、`AEMHeadless` クライアントをインスタンス化して React アプリ全体で使用するために書き出すので、最も重要です。

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## AEM GraphQL 永続クエリの実行の実装

汎用の `fetchPersistedQuery(..)` 関数を実装して AEM GraphQL 永続クエリを実行するには、`usePersistedQueries.js` ファイルを開きます。この `fetchPersistedQuery(..)` 関数は `aemHeadlessClient` オブジェクトの `runPersistedQuery()` 関数を使用して、promise ベースの動作でクエリを非同期で実行します。

後で、カスタムの React `useEffect` フックはこの関数を呼び出して、AEM から特定のデータを取得します。

1. `src/api/usePersistedQueries.js` の 35 行目、`fetchPersistedQuery(..)` を次のコードで&#x200B;**更新**&#x200B;します。

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

次に、React アプリのメインビューにチームとそのメンバーを表示する機能を構築します。この機能には、次が必要です。

- `src/api/usePersistedQueries.js` の新しい[カスタム React useEffect フック](https://react.dev/reference/react/useEffect#useeffect)は、`my-project/all-teams` 永続クエリを呼び出して、AEM のチームコンテンツフラグメントリストを返します。
- `src/components/Teams.js` にある React コンポーネントで、新しいカスタム React `useEffect` フックを呼び出し、チームデータをレンダリングします。

完了すると、アプリのメインビューに AEM のチームデータが入力されます。

![チーム表示](./assets/graphql-and-external-app/react-app__teams-view.png)

### 手順

1. `src/api/usePersistedQueries.js` を開きます。

1. 関数 `useAllTeams()` を探します

1. `fetchPersistedQuery(..)` を介して、永続クエリ `my-project/all-teams` を呼び出す `useEffect` フックを作成するには、次のコードを追加します。また、フックは AEM GraphQL レスポンスから `data?.teamList?.items` にある関連データのみを返すので、React の表示コンポーネントを親 JSON 構造に依存せずに済みます。

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

1. `Teams` React コンポーネントで、`useAllTeams()` フックを使用して AEM からチームのリストを取得します。

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

1. 最後に、チームデータをレンダリングします。GraphQL クエリから返された各チームは、提供された `Team` React サブコンポーネントを使用してレンダリングされます。

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


## 人物機能の実装

[チームの機能](#implement-teams-functionality)が完成したので、チームメンバーまたは個人の詳細表示を処理する機能を実装しましょう。

この機能には、次が必要です。

- `src/api/usePersistedQueries.js` の新しい[カスタム React useEffect フック](https://react.dev/reference/react/useEffect#useeffect)は、パラメータ化された `my-project/person-by-name` 永続クエリを呼び出し、単一の人物レコードを返します。

- `src/components/Person.js` にある React コンポーネントは、人物のフルネームをクエリパラメーターとして使用し、新しいカスタム React `useEffect` フックを呼び出して、人物データをレンダリングするものです。

完了したら、チーム表示で人物の名前を選択すると、人物表示がレンダリングされます。

![人物](./assets/graphql-and-external-app/react-app__person-view.png)

1. `src/api/usePersistedQueries.js` を開きます。

1. 関数 `usePersonByName(fullName)` を探します

1. `fetchPersistedQuery(..)` を介して、永続クエリ `my-project/all-teams` を呼び出す `useEffect` フックを作成するには、次のコードを追加します。また、フックは AEM GraphQL レスポンスから `data?.teamList?.items` にある関連データのみを返すので、React の表示コンポーネントを親 JSON 構造に依存せずに済みます。

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
1. `Person` React コンポーネントで、`fullName` ルートパラメーターを解析し、`usePersonByName(fullName)` フックを使用して AEM から人物データを取得します。

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

アプリ [http://localhost:3000/](http://localhost:3000/) をレビューし、_Members_ のリンクをクリックします。また、AEM でコンテンツフラグメントを追加することで、チームやメンバーを Team Alpha にさらに追加することもできます。

>[!IMPORTANT]
>
>実装の変更を検証する場合や、上記の変更後にアプリを動作させることができない場合は、[基本チュートリアル](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial)のソリューション分岐を参照してください。

## 内部の仕組み

ブラウザーの&#x200B;**開発者ツール**／**ネットワーク**&#x200B;を開いて、`all-teams` リクエストを&#x200B;_フィルター_&#x200B;します。GraphQL API リクエスト `/graphql/execute.json/my-project/all-teams` は、`REACT_APP_HOST_URI` の値に対して **ではなく** `http://localhost:3000` に対して行われることに注意してください（例：`<https://publish-pxxx-exxx.adobeaemcloud.com`）。リクエストは、React アプリのドメインに対して実行されます。[プロキシ設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)が `http-proxy-middleware` モジュールを使用して有効になっているからです。


![プロキシを介した GraphQL API リクエスト](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


メインの `../setupProxy.js` ファイルと `../proxy/setupProxy.auth.**.js` ファイル内を確認すると、`/content` と `/graphql` のパスがプロキシされ、静的アセットでないと示されていることがわかります。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

なお、ローカルプロキシの使用は、実稼動デプロイメントに適したオプションではありません。詳しくは、_実稼動デプロイメント_&#x200B;の節を参照してください。

## おめでとうございます。{#congratulations}

これですべて完了です。AEM の GraphQL API を使用してデータを消費および表示する React アプリを、基本チュートリアルの一部として正常に作成できました。
