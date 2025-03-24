---
title: React アプリを実装してユニバーサルエディターを使用してコンテンツを編集する
description: ユニバーサルエディターを使用してコンテンツを編集するために React アプリを実装する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 100%

---

# React アプリを実装してユニバーサルエディターを使用してコンテンツを編集する

ユニバーサルエディターを使用してコンテンツを編集するために React アプリを実装する方法について説明します。

## 前提条件

前の[ローカル開発の設定](./local-development-setup.md)手順で説明したように、ローカル開発環境が設定されています。

## ユニバーサルエディターコアライブラリを含める

まず、WKND Teams React アプリにユニバーサルエディターコアライブラリを含めましょう。これは、編集されたアプリとユニバーサルエディター間の通信レイヤーを提供する JavaScript ライブラリです。

React アプリにユニバーサルエディターコアライブラリを含める方法には、次の 2 つがあります。

1. npm レジストリからの Node モジュール依存関係については、[@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors) を参照してください。
1. HTMLファイル内のスクリプトタグ（`<script>`）。

このチュートリアルでは、スクリプトタグのアプローチを使ってみましょう。

1. `react-helmet-async` パッケージをインストールし、React アプリの `<script>` タグを管理します。

   ```bash
   $ npm install react-helmet-async
   ```

1. WKND Teams React アプリの `src/App.js` ファイルを更新して、ユニバーサルエディターコアライブラリを含めます。

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## メタデータの追加 - コンテンツのソース

WKND Teams React アプリを&#x200B;_コンテンツソースに_&#x200B;接続して編集するには、接続メタデータを指定する必要があります。ユニバーサルエディターサービスでは、このメタデータを使用してコンテンツのソースとの接続を確立します。

接続メタデータは、HTML ファイル内の `<meta>` タグとして保存されます。接続メタデータの構文は次のとおりです。

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

`<Helmet>` コンポーネント内の WKND Teams React アプリに接続メタデータを追加しましょう。次の `<meta>` タグを使用して `src/App.js`ファイルを更新します。この例では、コンテンツのソースは、`https://localhost:8443` で実行されているローカル AEM インスタンスです。

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

`aemconnection` では、コンテンツのソースの短い名前を指定します。後続の実装では、短い名前を使用してコンテンツのソースを参照します。

## メタデータの追加 - ローカルユニバーサルエディターサービスの設定

ローカル開発には、アドビがホストするユニバーサルエディターサービスの代わりに、ユニバーサルエディターサービスのローカルコピーが使用されます。ローカルサービスはユニバーサルエディターと AEM SDK をバインドするので、ローカルユニバーサルエディターサービスのメタデータを WKND Teams React アプリに追加しましょう。

また、これらの設定は、HTML ファイルの `<meta>` タグとしても保存されます。ローカルユニバーサルエディターサービスのメタデータの構文は次のとおりです。

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

`<Helmet>` コンポーネント内の WKND Teams React アプリに接続メタデータを追加しましょう。次の `<meta>` タグを使用して `src/App.js`ファイルを更新します。この例では、ローカルユニバーサルエディターサービスは、`https://localhost:8001` で実行されています。

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## React コンポーネントの実装

_チームのタイトルとチームの説明_&#x200B;など、WKND Teams React アプリのコンテンツを編集するには、React コンポーネントを実装する必要があります。実装とは、ユニバーサルエディターを使用して編集可能にする HTML 要素に関連するデータ属性（`data-aue-*`）を追加することを意味します。データ属性について詳しくは、[属性とタイプ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)を参照してください。

### 編集可能な要素の定義

まず、ユニバーサルエディターを使用して編集する要素を定義しましょう。WKND Teams React アプリでは、チームのタイトルと説明は AEM のチームコンテンツフラグメントに保存されるので、編集に最適です。

`Teams` React コンポーネントを実装して、チームのタイトルと説明を編集できるようにしましょう。

1. WKND Teams React アプリの `src/components/Teams.js` ファイルを開きます。
1. チームのタイトルと説明の要素に、`data-aue-prop`、`data-aue-type` および `data-aue-label` 属性を追加します。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. WKND Teams React アプリを読み込むブラウザーでユニバーサルエディターページを更新します。これで、チームのタイトルと説明の要素が編集可能であることが確認できます。

   ![ユニバーサルエディター - WKND Teams のタイトルと説明が編集可能](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. インライン編集またはプロパティパネルを使用してチームのタイトルまたは説明を編集しようとすると、読み込みスピナーが表示されますが、コンテンツを編集できません。ユニバーサルエディターは、コンテンツの読み込みと保存に関する AEM リソースの詳細を認識していないからです。

   ![ユニバーサルエディター - WKND Teams のタイトルと説明の読み込み](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

要約すると、上記の変更により、チームのタイトルと説明の要素がユニバーサルエディターで編集可能としてマークされます。ただし、**まだ変更を編集して（インラインまたはプロパティパネル経由で）保存することはできません**。そのためには、`data-aue-resource` 属性を使用して AEM リソースの詳細を追加する必要があります。次の手順で実行しましょう。

### AEM リソースの詳細の定義

編集したコンテンツを AEM に保存し直し、プロパティパネルにコンテンツを読み込むには、AEM リソースの詳細をユニバーサルエディターに指定する必要があります。

この場合、AEM リソースはチームコンテンツフラグメントパスなので、最上位レベルの `<div>` 要素にある `Teams` React コンポーネントにリソースの詳細を追加しましょう。

1. `src/components/Teams.js` ファイルを更新して、最上位の `<div>` 要素に `data-aue-resource`、`data-aue-type` および `data-aue-label` 属性を追加します。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   `data-aue-resource` 属性の値は、チームコンテンツフラグメントの AEM リソースパスです。`urn:aemconnection:` プレフィックスでは、接続メタデータで定義されたコンテンツのソースの短い名前を使用します。

1. WKND Teams React アプリを読み込むブラウザーでユニバーサルエディターページを更新します。これで、最上位の Team 要素は編集可能ですが、プロパティパネルにはまだコンテンツが読み込まれていないことが確認できます。ブラウザーのネットワークタブでは、コンテンツを読み込む `details` リクエストに対して 401 Unauthorized エラーが表示されます。認証に IMS トークンを使用しようとしていますが、ローカル AEM SDK は IMS 認証をサポートしていません。

   ![ユニバーサルエディター - WKND Teams チームが編集可能](./assets/universal-editor-wknd-teams-team-editable.png)

1. 401 Unauthorized エラーを修正するには、ユニバーサルエディターの「**認証ヘッダー**」オプションを使用して、ローカル AEM SDK 認証の詳細をユニバーサルエディターに指定する必要があります。ローカル AEM SDK と同様に、`admin:admin` 資格情報の値を `Basic YWRtaW46YWRtaW4=` に設定します。

   ![ユニバーサルエディター - 認証ヘッダーの追加](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. WKND Teams React アプリを読み込むブラウザーでユニバーサルエディターページを更新します。これで、プロパティパネルにコンテンツが読み込まれ、チームのタイトルと説明をインラインで、またはプロパティパネルを使用して編集できることが確認できます。

   ![ユニバーサルエディター - WKND Teams チームが編集可能](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### 内部の仕組み

プロパティパネルでは、ローカルユニバーサルエディターサービスを使用して AEM リソースからコンテンツを読み込みます。ブラウザーのネットワークタブを使用すると、コンテンツを読み込むためのローカルユニバーサルエディターサービス（`https://localhost:8001/details`）に対する POST リクエストを確認できます。

インライン編集またはプロパティパネルを使用してコンテンツを編集すると、変更内容はローカルユニバーサルエディターサービスを使用して AEM リソースに保存されます。ブラウザーのネットワークタブを使用すると、コンテンツを保存するためのローカルユニバーサルエディターサービス（`https://localhost:8001/update` または `https://localhost:8001/patch`）に対する POST リクエストを確認できます。

![ユニバーサルエディター - WKND Teams チームが編集可能](./assets/universal-editor-under-the-hood-request.png)

リクエストペイロード JSON オブジェクトには、コンテンツサーバー（`connections`）、リソースパス（`target`）、更新されたコンテンツ（`patch`）などの必要な詳細が含まれます。

![ユニバーサルエディター - WKND Teams チームが編集可能](./assets/universal-editor-under-the-hood-payload.png)

### 編集可能なコンテンツの展開

編集可能なコンテンツを展開し、実装を&#x200B;**チームメンバー**&#x200B;に適用し、プロパティパネルを使用してチームメンバーを編集できるようにしましょう。

上記のように、`Teams` React コンポーネントのチームメンバーに関連する `data-aue-*` 属性を追加しましょう。

1. `src/components/Teams.js` ファイルを更新して、`<li key={index} className="team__member">` 要素にデータ属性を追加します。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   チームメンバーは AEM に `Person` コンテンツフラグメントとして保存されるので、`data-aue-type` 属性の値は `component` であり、コンテンツの移動可能／削除可能な部分を示すのに役立ちます。

1. WKND Teams React アプリを読み込むブラウザーでユニバーサルエディターページを更新します。これで、プロパティパネルを使用してチームメンバーは編集可能であることが確認できます。

   ![ユニバーサルエディター - WKND Teams チームメンバーが編集可能](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### 内部の仕組み

上記のように、コンテンツの取得と保存はローカルユニバーサルエディターサービスによって実行されます。`/details`、`/update` または `/patch` リクエストは、コンテンツの読み込みと保存のためにローカルユニバーサルエディターサービスに対して行われます。

### コンテンツの追加および削除の定義

ここまでで、既存のコンテンツを編集可能にしてきました。では、新しいコンテンツを追加するにはどうすればよいしょうか？ユニバーサルエディターを使用して、WKND チームにチームメンバーを追加または削除する機能を追加しましょう。したがって、コンテンツ作成者は、AEM に移動しなくてもチームメンバーを追加または削除できます。

ただし、簡単にまとめると、WKND チームメンバーは AEM に `Person` コンテンツフラグメントとして保存され、`teamMembers` プロパティを使用してチームコンテンツフラグメントに関連付けられます。AEM のモデル定義を確認するには、[my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project) を参照してください。

1. まず、コンポーネント定義ファイル `/public/static/component-definition.json` を作成します。このファイルには、`Person` コンテンツフラグメントのコンポーネント定義が含まれます。`aem/cf` プラグインを使用すると、適用するデフォルト値を指定するモデルとテンプレートに基づいて、コンテンツフラグメントを挿入できます。

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. 次に、WKND Teams React アプリの `index.html` にある上記のコンポーネント定義ファイルを参照します。コンポーネント定義ファイルを含めるように、`public/index.html`ファイルの `<head>` セクションを更新します。

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. 最後に、`src/components/Teams.js` ファイルを更新して、データ属性を追加します。チームメンバーのコンテナとして機能する「**MEMBERS**」セクションでは、`<div>` 要素に `data-aue-prop`、`data-aue-type`、`data-aue-label` 属性を追加しましょう。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. WKND Teams React アプリを読み込むブラウザーでユニバーサルエディターページを更新します。これで、「**MEMBERS**」セクションがコンテナとして機能することが確認できます。プロパティパネルと **+** アイコンを使用して、新しいチームメンバーを挿入できます。

   ![ユニバーサルエディター - WKND Teams チームメンバーが挿入](./assets/universal-editor-wknd-teams-add-team-members.png)

1. チームメンバーを削除するには、チームメンバーを選択し、**削除**&#x200B;アイコンをクリックします。

   ![ユニバーサルエディター - WKND Teams チームメンバーが削除](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### 内部の仕組み

コンテンツの追加および削除操作は、ローカルユニバーサルエディターサービスによって実行されます。AEM にコンテンツを追加または削除するために、詳細なペイロードを含む `/add` または `/remove` への POST リクエストがローカルユニバーサルエディターサービスに対して行われます。

## ソリューションファイル

実装の変更を検証する場合や、WKND Teams React アプリをユニバーサルエディターで動作させることができない場合は、[basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) ソリューション分岐を参照してください。

作業用 **basic-tutorial** 分岐とのファイル別の比較は、[こちら](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1)を参照してください。

## これで完了です

ユニバーサルエディターを使用してコンテンツを追加、編集、削除するための WKND Teams React アプリが正常に実装されました。コアライブラリを含める方法、接続とローカルユニバーサルエディターサービスのメタデータを追加する方法、様々なデータ（`data-aue-*`）属性を使用して React コンポーネントを実装する方法について説明しました。
