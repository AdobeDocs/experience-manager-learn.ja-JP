---
title: クライアントアプリケーションの統合 — AEMヘッドレスの高度な概念 — GraphQL
description: 永続的なクエリを実装し、WKND アプリに統合します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 1%

---


# クライアントアプリケーションの統合

前の章では、HTTP クエリとPOSTリクエストを使用して、永続化されたクエリを作成および更新しました。

この章では、5 つの React コンポーネント内で HTTPGETリクエストを使用して、これらの永続化されたクエリを WKND アプリに統合する手順について説明します。

* 場所
* アドレス
* 講師
* 管理者
* チーム

## 前提条件 {#prerequisites}

このドキュメントは、マルチパートチュートリアルの一部です。 この章を進める前に、前の章が完了していることを確認してください。 の完了 [基本チュートリアル](/help/headless-tutorial/graphql/multi-step/overview.md) をお勧めします。

_この章の IDE スクリーンショットは、次のものから取得します。 [Visual Studio Code](https://code.visualstudio.com/)_

### 第 1 章～第 4 章ソリューションパッケージ（任意） {#solution-package}

1 ～ 4 章のAEM UI の手順を完了するソリューションパッケージがインストール可能です。 このパッケージは **不要** 前の章が完了している場合

1. ダウンロード [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip).
1. AEMで、に移動します。 **ツール** > **導入** > **パッケージ** アクセスする **パッケージマネージャー**.
1. 前の手順でダウンロードしたパッケージ（zip ファイル）をアップロードしてインストールします。

## 目的 {#objectives}

このチュートリアルでは、AEMヘッドレス JavaScript を使用して、永続的なクエリのリクエストをサンプル WKND GraphQl React アプリに統合する方法を学びます [SDK](https://github.com/adobe/aem-headless-client-js).

## サンプルクライアントアプリケーションをインストールして実行します。 {#install-client-app}

チュートリアルを高速化するために、スターター React JS アプリが提供されます。

>[!NOTE]
> 
> 以下の手順は、React アプリを **作成者** を使用してas a Cloud ServiceするAEMの環境 [ローカル開発アクセストークン](/help/headless-tutorial/authentication/local-development-access-token.md). アプリを [AEMaaCS SDK を使用したローカルオーサーインスタンス](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 基本認証を使用します。

1. ダウンロード **[aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)**.
1. ファイルを解凍し、IDE でプロジェクトを開きます。
1. の取得 [ローカル開発トークン](/help/headless-tutorial/authentication/local-development-access-token.md) (target AEM環境用 )
1. プロジェクトで、ファイルを開きます。 `.env.development`.
   1. 設定 `REACT_APP_DEV_TOKEN` 次と等しい `accessToken` の値をローカル開発トークンから取得します。 （JSON ファイル全体ではありません）
   1. 設定 `REACT_APP_HOST_URI` AEMの url に **作成者** 環境。

   ![ローカル環境ファイルを更新](assets/client-application-integration/update-environment-file.png)
1. 新しいターミナルを開き、プロジェクトフォルダーに移動します。 次のコマンドを実行します。

   ```shell
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーが次の場所で開きます。 `http://localhost:3000/aem-guides-wknd-pwa`.
1. タップ **Camping** > **ヨセミテバックパッキン** ヨセミテバックパッキングの冒険の詳細を見る

   ![Yosemite バックパッキング画面](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. ブラウザーの開発者ツールを開き、 `XHR` リクエスト

   ![POSTGraphQL](assets/client-application-integration/post-query-graphql.png)

   次のように表示されます。 `POST` を GraphQL エンドポイントに送信します。 の表示 `Payload`を見ると、送信された完全な GraphQL クエリを確認できます。 次の節では、アプリが更新されてを使用するようになります。 **持続** クエリ。


## 概要

基本チュートリアルでは、パラメーター化された GraphQl クエリを使用して、単一のコンテンツフラグメントを要求し、アドベンチャーの詳細をレンダリングします。 次に、 `adventureDetailQuery` 新しいフィールドを含め、前の章で作成した永続クエリを使用するには、次の手順に従います。

次の 5 つのコンポーネントが作成されます。

| React コンポーネント | 場所 |
|-------|------|
| 管理者 | `src/components/Administrator.js` |
| チーム | `src/components/Team.js` |
| 場所 | `src/components/Location.js` |
| 講師 | `src/components/Instructors.js` |
| アドレス | `src/components/Address.js` |

## useGraphQL フックを更新

カスタム [React エフェクトフック](https://reactjs.org/docs/hooks-overview.html#effect-hook) アプリの `query`を呼び出し、変更するとAEM GraphQL エンドポイントに HTTPPOSTリクエストを送信し、アプリに JSON 応答を返します。

使用する新しいフックを作成します **持続** クエリ。 その後、アプリはアドベンチャーの詳細に対して HTTPGETリクエストを実行できます。 この `runPersistedQuery` の [AEM Headless Client SDK](https://github.com/adobe/aem-headless-client-js) を使用して、永続化されたクエリの実行を容易にします。

1. ファイルを開きます。 `src/api/useGraphQL.js`
1. 新しいフックを追加 `useGraphQLPersisted`:

   ```javascript
   /**
   * Custom React Hook to perform a GraphQL query to a persisted query endpoint
   * @param persistedPath - the short path to the persisted query
   * @param fragmentPathParam - optional parameters object that can be passed in for parameterized persistent queries
   */
   export function useGraphQLPersisted(persistedPath, fragmentPathVariable) {
       let [data, setData] = useState(null);
       let [errors, setErrors] = useState(null);
   
       useEffect(() => {
           let queryVariables = {};
   
           // we pass in a primitive fragmentPathVariable (String) and then construct the object {fragmentPath: fragmentPathParam} to pass as query params to the persisted query
           // It is simpler to pass a primitive into a React hooks, as comparing the state of a dependent object can be difficult. see https://reactjs.org/docs/hooks-faq.html#can-i-skip-an-effect-on-updates
           if(fragmentPathVariable) {
               queryVariables = {fragmentPath: fragmentPathVariable};
           }
   
           // execute a persisted query using the given path and pass in variables (if needed)
           sdk.runPersistedQuery(persistedPath, queryVariables)
               .then(({ data, errors }) => {
               if (errors) setErrors(mapErrors(errors));
               if (data) setData(data);
           })
           .catch((error) => {
           setErrors(error);
           });
   }, [persistedPath, fragmentPathVariable]);
   
   return { data, errors }
   }
   ```
1. ファイルに変更を保存します。

## アドベンチャー詳細コンポーネントを更新

ファイル `src/api/queries.js` には、アプリケーションを強化するために使用される GraphQL クエリが含まれます `adventureDetailQuery` 個々のアドベンチャーの詳細を、標準のPOSTGraphQL リクエストを使用して返します。 次に、 `AdventureDetail` 持続的なを使用するコンポーネント `wknd/all-adventure-details` クエリ。

1. 次を開きます： `src/screens/AdventureDetail.js`.
1. 最初に次の行をコメントアウトします。

   ```javascript
   export default function AdventureDetail() {
   
       ...
   
       //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
   ```

   上記では、標準の GraphQLPOSTを使用し、 `adventureFragmentPath`

1. 次の手順で `useGraphQLPersisted` フック、次の行を追加します。

   ```javascript
   export default function AdventureDetail() {
   
      //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
       const {data, errors} = useGraphQLPersisted("wknd/all-adventure-details", adventureFragmentPath);
   ```

   パスを観察します。 `wknd/all-adventure-details` は、前の章で作成した永続化されたクエリのパスです。

   >[!CAUTION]
   >
   > 更新されたクエリが `wknd/all-adventure-details` は、target AEM環境で保持する必要があります。 の手順を確認します。 [永続的な GraphQL クエリ](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md#cache-control-all-adventures) または [AEMソリューションパッケージ](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)

1. ブラウザーで実行中のアプリに戻り、ブラウザーの開発者ツールを使用して、 **アドベンチャーの詳細** ページ。

   ![リクエストを取得](assets/client-application-integration/get-request-persisted-query.png)

   ```
   http://localhost:3000/graphql/execute.json/wknd/all-adventure-details;fragmentPath=/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking
   ```

   これで、 `GET` 次の場所にある永続化されたクエリを使用するリクエスト `wknd/all-adventure-details`.

1. 他の冒険の詳細に移動し、同じことを確認します。 `GET` リクエストが作成されますが、異なるフラグメントパスが使用されます。 アプリケーションは、引き続き以前と同じように動作します。

参照： `AdventureDetail.js` 内 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 更新されたコンポーネントの完全な例を示します。

次に、 **場所**, **管理者**、および **講師** 位置データをレンダリングするコンポーネント この **住所** コンポーネントが **チーム** コンポーネント。

## 場所コンポーネントの開発

1. 内 `AdventureDetail.js` ファイルを開き、 `<Location>` から位置データを渡すコンポーネント `adventure` データオブジェクト：

   ```javascript
   export default function AdventureDetail() {
       ...
   
       return (
           ...
   
           <Location data={adventure.location} />
   ```

1. 次の場所にあるファイルを確認します。 `src/components/Location.js`. この `Location` コンポーネントは、満たすべき場所、連絡先情報、天気に関する情報および **場所** コンテンツフラグメントモデル。 少なくとも、 `Location` コンポーネントは、 `address` 渡すオブジェクト。
1. 参照： `Location.js` 内 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 更新されたコンポーネントの完全な例を示します。

更新が完了すると、レンダリングされた詳細ページは次のようになります。

![location-component](assets/client-application-integration/location-component.png)

## チームコンポーネントの開発

1. 内 `AdventureDetail.js` ファイルを開き、 `<Team>` コンポーネント ( `<Location>` コンポーネント ) を渡す `instructorTeam` データ `adventure` データオブジェクト：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   ```

1. 次の場所にあるファイルを確認します。 `src/components/Team.js`. この `Team` コンポーネントは、チームの作成日、画像、説明に関するデータを **チーム** コンテンツフラグメント。

1. In `Team.js` この `Address` コンポーネント。

   ```javascript
   export default function Team({data}) {
       ...
       {teamPath && <Address _path={teamPath}/>}
   ```

   ここで、現在のチームへのパスが `Address` コンポーネント。次に、クエリを実行して、チームに基づいてアドレスを取得します。

1. 参照： `Team.js` 内 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) コンポーネントの完全な例を示します。

クエリを統合すると、次のようになります。

![team-component](assets/client-application-integration/address-component.png)

## アドレスコンポーネントの開発

1. 次の場所にあるファイルを確認します。 `src/components/Address.js`. この `Address` 住所情報（番地、市区町村、都道府県、郵便番号、国など）を **住所** コンテンツフラグメントと、 **連絡先情報** フラグメント参照。
1. この `Address` コンポーネントは、 `AdventureDetails` コンポーネントを使用し、パスに基づいてデータを取得する永続的な呼び出しをおこないます。 違いは、 `/wknd/team-location-by-location-path` をクリックしてリクエストを実行します。
1. 参照： `Address.js` 内 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) コンポーネントの完全な例を示します。

## 管理者コンポーネントの開発

1. 内 `AdventureDetail.js` ファイルを開き、 `<Adminstrator>` コンポーネント ( `<Team>` コンポーネント ) を渡す `administrator` データ `adventure` データオブジェクト：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   <Administrator data={adventure.administrator} /> 
   ```

1. 次の場所にあるファイルを確認します。 `src/components/Administrator.js`. この `Administrator` コンポーネントは、詳細（完全名など）を **管理者** コンテンツフラグメントを作成し、 **連絡先情報** フラグメント参照。
1. 参照： `Administrator.js` in [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) コンポーネントの完全な例を示します。

管理者コンポーネントを作成したら、アプリケーションをレンダリングする準備が整いました。 出力は、以下の画像と一致する必要があります。

![administrator-component](assets/client-application-integration/administrator-component.png)

## Instructors コンポーネントの開発

1. 内 `AdventureDetail.js` ファイルを開き、 `<Instructors>` コンポーネント ( `<Administrator>` コンポーネント ) を渡す `instructorTeam` データ `adventure` データオブジェクト：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam}/>
   <Administrator data={adventure.administrator} />             
   <Instructors data={adventure.instructorTeam} />
   ```

1. 次の場所にあるファイルを確認します。 `src/components/Instructors.js`. この `Instructors` コンポーネントは、チームの各メンバーに関するデータをレンダリングします（氏名、伝記、画像、電話番号、経験レベル、スキルなど）。 このコンポーネントは、配列を反復して各メンバーを表示します。
1. 参照： `Instructors.js` in [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) コンポーネントの完全な例を示します。

アプリケーションをレンダリングすると、出力は次の画像と一致する必要があります。

![instructors-component](assets/client-application-integration/instructors-component.png)

## 完成したサンプル WKND アプリ

完成したアプリは次のようになります。

![AEM-headless-final-experience](assets/client-application-integration/aem-headless-final-experience.gif)

### 最終クライアントアプリケーション

アプリの最終バージョンは、次のようにダウンロードして使用できます。
**[aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)**

## これで完了です

おめでとうございます。これで、統合が完了し、永続化されたクエリをサンプル WKND アプリに実装することが完了しました。