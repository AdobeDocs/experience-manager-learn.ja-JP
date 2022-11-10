---
title: クライアントアプリケーションの統合 — AEMヘッドレスの高度な概念 — GraphQL
description: 永続的なクエリを実装し、WKND アプリに統合します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 2%

---

# クライアントアプリケーションの統合

前の章では、GraphiQL エクスプローラーを使用して永続クエリを作成し、更新しました。

この章では、既存の内部で HTTPGETリクエストを使用して、永続化されたクエリを WKND クライアントアプリケーション（WKND アプリ）に統合する手順について説明します **React コンポーネント**. また、AEMヘッドレスラーニングを適用し、WKND クライアントアプリケーションを強化するための専門知識をコーディングするためのオプションの課題も提供します。

## 前提条件 {#prerequisites}

このドキュメントは、マルチパートチュートリアルの一部です。 この章を進める前に、前の章が完了していることを確認してください。 WKND クライアントアプリケーションはAEMパブリッシュサービスに接続するので、次の操作をおこなうことが重要です。 **次の内容をAEMパブリッシュサービスに公開しました**.

* プロジェクト設定
* GraphQL エンドポイント
* コンテンツフラグメントモデル
* 作成済みコンテンツフラグメント
* GraphQL 永続クエリ

この _この章の IDE スクリーンショットは、次のものから取得します。 [Visual Studio Code](https://code.visualstudio.com/)_

### 第 1 章～第 4 章ソリューションパッケージ（任意） {#solution-package}

1 ～ 4 章のAEM UI の手順を完了するソリューションパッケージがインストール可能です。 このパッケージは **不要** 前の章が完了している場合

1. ダウンロード [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. AEMで、に移動します。 **ツール** > **導入** > **パッケージ** アクセスする **パッケージマネージャー**.
1. 前の手順でダウンロードしたパッケージ（zip ファイル）をアップロードしてインストールします。
1. AEM パブリッシュサービスにパッケージをレプリケートします

## 目的 {#objectives}

このチュートリアルでは、を使用して、永続的なクエリのリクエストをサンプル WKND GraphQL React アプリに統合する方法を学びます。 [JavaScript 用AEMヘッドレスクライアント](https://github.com/adobe/aem-headless-client-js).

## サンプルクライアントアプリケーションの複製と実行 {#clone-client-app}

チュートリアルを高速化するために、スターター React JS アプリが提供されます。

1. のクローン [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. を編集します。 `aem-guides-wknd-graphql/advanced-tutorial/.env.development` ファイルとセット `REACT_APP_HOST_URI` をクリックして、target AEMパブリッシュサービスを指すように設定します。

   オーサーインスタンスに接続する場合は、認証方法を更新します。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![React アプリ開発環境](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > 上記の手順は、React アプリを **AEM パブリッシュサービス**&#x200B;ただし、 **AEM オーサーサービス** target AEM as a Cloud Service環境のローカル開発トークンを取得します。
   >
   > アプリを [AEMaaCS SDK を使用したローカルオーサーインスタンス](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 基本認証を使用します。


1. ターミナルを開き、次のコマンドを実行します。

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウが読み込まれます。 [http://localhost:3000](http://localhost:3000)


1. タップ **Camping** > **ヨセミテバックパッキン** ヨセミテバックパッキングの冒険の詳細を見る

   ![Yosemite バックパッキング画面](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. ブラウザーの開発者ツールを開き、 `XHR` リクエスト

   ![POSTGraphQL](assets/client-application-integration/graphql-persisted-query.png)

   次のようになります。 `GET` プロジェクト設定名 (`wknd-shared`)、永続化されたクエリ名 (`adventure-by-slug`)，変数名 (`slug`)，値 (`yosemite-backpacking`)、および特殊文字エンコーディングで使用できます。

>[!IMPORTANT]
>
>    GraphQL API リクエストが `http://localhost:3000` AEM パブリッシュサービスドメインに対してではなく、 [フードの下で](../multi-step/graphql-and-react-app.md#under-the-hood) 基本チュートリアルから。


## コードの確認

内 [基本チュートリアル — AEM GraphQL API を使用した React アプリの構築](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 実践的な専門知識を得るために、主要なファイルをいくつか確認し、拡張した手順です。 WKND アプリを強化する前に、主要なファイルを確認します。

* [AEMHeadless オブジェクトを確認する](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [AEM GraphQL 永続クエリを実行するための実装](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### レビュー `Adventures` React コンポーネント

WKND React アプリのメインビューは、すべてのアドベンチャのリストです。これらのアドベンチャは、次のようなアクティビティタイプに基づいてフィルタリングできます。 _キャンピング、サイクリング_. このビューは、 `Adventures` コンポーネント。 主な実装の詳細を次に示します。

* この `src/components/Adventures.js` 呼び出し `useAllAdventures(adventureActivity)` ここに逃げ込む `adventureActivity` 引数はアクティビティタイプです。

* この `useAllAdventures(adventureActivity)` フックが `src/api/usePersistedQueries.js` ファイル。 基準 `adventureActivity` の値を指定する場合、呼び出す永続クエリを決定します。 null 値でない場合は、を呼び出します。 `wknd-shared/adventures-by-activity`を指定しない場合は、利用可能なすべてのアドベンチャーを取得します `wknd-shared/adventures-all`.

* フックはメイン `fetchPersistedQuery(..)` クエリ実行をに委任する関数 `AEMHeadless` 経由 `aemHeadlessClient.js`.

* また、関連するデータは、AEM GraphQL 応答から、 `response.data?.adventureList?.items`、 `Adventures` 親 JSON 構造に依存しない React ビューコンポーネント。

* クエリが正常に実行されると、 `AdventureListItem(..)` 関数を `Adventures.js` を表示するHTML要素を追加します。 _画像、旅行の長さ、価格、タイトル_ 情報。

### レビュー `AdventureDetail` React コンポーネント

この `AdventureDetail` React コンポーネントは、アドベンチャーの詳細をレンダリングします。 主な実装の詳細を次に示します。

* この `src/components/AdventureDetail.js` 呼び出し `useAdventureBySlug(slug)` ここに逃げ込む `slug` 引数はクエリパラメータです。

* 上記のように、 `useAdventureBySlug(slug)` フックが `src/api/usePersistedQueries.js` ファイル。 呼び出し `wknd-shared/adventure-by-slug` に委任することで、永続化されたクエリを `AEMHeadless` 経由 `aemHeadlessClient.js`.

* クエリが正常に実行されると、 `AdventureDetailRender(..)` 関数を `AdventureDetail.js` アドベンチャーの詳細を表示するHTML要素を追加します。


## コードの拡張

### 用途 `adventure-details-by-slug` 持続クエリ

前の章では、 `adventure-details-by-slug` 持続的なクエリ。次のような追加のアドベンチャー情報を提供します。 _場所、講師チーム、管理者_. 次を置き換えます。 `adventure-by-slug` と `adventure-details-by-slug` 永続化されたクエリを使用して、この追加情報をレンダリングできます。

1. `src/api/usePersistedQueries.js`を開きます。

1. 関数の場所 `useAdventureBySlug()` クエリを更新

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### 追加情報を表示

1. 追加のアドベンチャー情報を表示するには、 `src/components/AdventureDetail.js`

1. 関数の場所 `AdventureDetailRender(..)` として戻り関数を更新

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. 対応するレンダリング関数も定義します。

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **場所**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **管理者**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### 新しいスタイルを定義

1. 開く `src/components/AdventureDetail.scss` 次のクラス定義を追加します。

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>更新されたファイルは、以下で入手できます。 **AEMガイド WKND - GraphQL** プロジェクト、詳しくは、 [高度なチュートリアル](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 」セクションに入力します。


上記の機能強化が完了すると、WKND アプリは以下のようになり、ブラウザーの開発者ツールが表示します `adventure-details-by-slug` 永続的なクエリ呼び出し。

![拡張 WKND アプリ](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 機能強化の課題（オプション）

WKND React アプリのメインビューでは、次のようなアクティビティタイプに基づいてこれらの Adventures をフィルタリングできます。 _キャンピング、サイクリング_. しかし、WKND ビジネスチームは追加の _場所_ ベースのフィルタリング機能 使用するための要件は以下のとおりです。

* WKND アプリのメイン表示で、左上隅または右隅に _場所_ フィルターアイコン。
* クリック _場所_ フィルターアイコンには、場所のリストが表示されます。
* リストから目的のロケーションオプションをクリックすると、一致する Adventures のみが表示されます。
* 一致するアドベンチャーが 1 つだけの場合、アドベンチャーの詳細ビューが表示されます。

## これで完了です

おめでとうございます。これで、統合が完了し、永続化されたクエリをサンプル WKND アプリに実装することが完了しました。
