---
title: React アプリを実装して、ユニバーサルエディターを使用してコンテンツを編集する
description: ユニバーサルエディターを使用してコンテンツを編集するように React アプリを実装する方法を説明します。
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---

# React アプリを実装して、ユニバーサルエディターを使用してコンテンツを編集する

ユニバーサルエディターを使用してコンテンツを編集するように React アプリを実装する方法を説明します。

## 前提条件

前の [ローカル開発設定 ](./local-development-setup.md) 手順の説明に従って、ローカル開発環境が設定されていること。

## ユニバーサルエディターのコアライブラリを含める

まず、WKND Teams の React アプリにユニバーサルエディターのコアライブラリを含めます。 これは、編集されたアプリとユニバーサルエディターの間の通信レイヤーを提供するJavaScript ライブラリです。

ユニバーサルエディターのコアライブラリを React アプリに含める方法は 2 とおりあります。

1. npm レジストリからのノードモジュール依存関係については、[@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors) を参照してください。
1. HTMLファイル内のスクリプトタグ（`<script>`）。

このチュートリアルでは、スクリプトタグのアプローチを使用します。

1. `react-helmet-async` パッケージをインストールして、React アプリで `<script>` タグを管理します。

   ```bash
   $ npm install react-helmet-async
   ```

1. WKND Teams React アプリの `src/App.js` ファイルを更新して、ユニバーサルエディターのコアライブラリを含めます。

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

## メタデータの追加 – コンテンツソース

編集のために WKND チーム React アプリ _コンテンツソースを使用_ を接続するには、接続メタデータを指定する必要があります。 ユニバーサルエディターサービスは、このメタデータを使用してコンテンツソースとの接続を確立します。

接続メタデータは、HTMLファイル `<meta>` タグとして保存されます。 接続メタデータの構文は次のとおりです。

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

`<Helmet>` コンポーネント内の WKND Teams React アプリに接続メタデータを追加します。 次の `<meta>` タグで `src/App.js` ファイルを更新します。 この例では、コンテンツソースは、`https://localhost:8443` 上で動作するローカル AEM インスタンスです。

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

`aemconnection` は、コンテンツソースの短い名前を提供します。 その後のインストルメンテーションでは、短い名前を使用してコンテンツソースを参照します。

## メタデータの追加 – ローカルユニバーサルエディターサービスの設定

Adobeにホストされたユニバーサルエディターサービスの代わりに、ユニバーサルエディターサービスのローカルコピーがローカル開発に使用されます。 ローカルサービスはユニバーサルエディターとAEM SDK をバインドするので、ローカルのユニバーサルエディターサービスのメタデータを WKND Teams React アプリに追加します。

これらの設定は、HTMLファイルの `<meta>` タグとしても保存されます。 ローカルのユニバーサルエディターサービスのメタデータの構文は、次のとおりです。

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

`<Helmet>` コンポーネント内の WKND Teams React アプリに接続メタデータを追加します。 次の `<meta>` タグで `src/App.js` ファイルを更新します。 この例では、ローカルのユニバーサルエディターサービスが `https://localhost:8001` 上で実行されています。

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

_チームのタイトルとチームの説明_ など、WKND チームの React アプリのコンテンツを編集するには、React コンポーネントを実装する必要があります。 インストルメンテーションとは、ユニバーサルエディターを使用して編集可能にするHTML要素に、関連するデータ属性（`data-aue-*`）を追加することを意味します。 データ属性の詳細については、[ 属性と型 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types) を参照してください。

### 編集可能な要素の定義

まず、ユニバーサルエディターを使用して編集する要素を定義します。 WKND Teams の React アプリでは、チームのタイトルと説明がAEMの Team コンテンツフラグメントに保存されるので、最も適した編集候補になります。

`Teams` React コンポーネントを実装して、チームのタイトルと説明を編集可能にしましょう。

1. WKND Teams React アプリの `src/components/Teams.js` ファイルを開きます。
1. `data-aue-prop`、`data-aue-type`、`data-aue-label` 属性をチームのタイトルと説明の要素に追加します。

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

1. WKND Teams React アプリを読み込むブラウザーで、ユニバーサルエディターページを更新します。 これで、チームのタイトルと説明の要素が編集可能であることが確認できます。

   ![ ユニバーサルエディター – WKND チームのタイトルと説明を編集可能 ](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. インライン編集またはプロパティパネルを使用してチームのタイトルや説明を編集しようとすると、読み込みスピナーが表示されますが、コンテンツを編集できません。 ユニバーサルエディターは、コンテンツの読み込みと保存のためのAEM リソースの詳細を認識しないためです。

   ![ ユニバーサルエディター – WKND Teams のタイトルと説明の読み込み ](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

まとめると、上記の変更により、チームのタイトルと説明の要素がユニバーサルエディターで編集可能としてマークされます。 ただし、**インラインまたはプロパティパネル経由で）編集して変更を保存することはまだできません**。そのため、`data-aue-resource` 属性を使用してAEM リソースの詳細を追加する必要があります。 次の手順でそれを行いましょう。

### AEM リソースの詳細を定義

編集したコンテンツをAEMに保存し直し、プロパティパネルにコンテンツを読み込むには、AEM リソースの詳細をユニバーサルエディターに指定する必要があります。

この場合、AEM リソースは Team コンテンツフラグメントパスなので、リソースの詳細を最上位レベルの `<div>` 要素の `Teams` React コンポーネントに追加します。

1. `src/components/Teams.js` ファイルを更新して、`data-aue-resource`、`data-aue-type` および `data-aue-label` 属性を最上位の `<div>` 要素に追加します。

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

   `data-aue-resource` 属性の値は、Team コンテンツフラグメントのAEM リソースパスです。 `urn:aemconnection:` プレフィックスには、接続メタデータで定義されたコンテンツ ソースの短い名前を使用します。

1. WKND Teams React アプリを読み込むブラウザーで、ユニバーサルエディターページを更新します。 最上位のチーム要素が編集可能になっていますが、プロパティパネルがまだコンテンツを読み込んでいません。 ブラウザーの「ネットワーク」タブには、コンテンツを読み込む `details` リクエストに対する 401 Unauthorized エラーが表示されます。 認証に IMS トークンを使用しようとしていますが、ローカルのAEM SDK は IMS 認証をサポートしていません。

   ![ ユニバーサルエディター – WKND チームチームを編集可能 ](./assets/universal-editor-wknd-teams-team-editable.png)

1. 401 Unauthorized エラーを修正するには、ユニバーサルエディターで「**認証ヘッダー**」オプションを使用して、ローカル AEM SDK 認証の詳細をユニバーサルエディターに指定する必要があります。 ローカルのAEM SDK として、`admin:admin` 資格情報の値を `Basic YWRtaW46YWRtaW4=` に設定します。

   ![ ユニバーサルエディター – 認証ヘッダーの追加 ](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. WKND Teams React アプリを読み込むブラウザーで、ユニバーサルエディターページを更新します。 プロパティパネルにコンテンツが読み込まれ、チームのタイトルと説明をインラインで編集するか、プロパティパネルを使用することが可能になりました。

   ![ ユニバーサルエディター – WKND チームチームを編集可能 ](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### 内部で

プロパティパネルは、ローカルのユニバーサルエディターサービスを使用して、AEM リソースからコンテンツを読み込みます。 ブラウザーの「ネットワーク」タブを使用すると、コンテンツの読み込みのためにローカルのユニバーサルエディターサービス（`https://localhost:8001/details`）に対するPOSTリクエストを確認できます。

インライン編集またはプロパティパネルを使用してコンテンツを編集すると、変更内容がローカルのユニバーサルエディターサービスを使用してAEM リソースに保存されます。 ブラウザーの「Network」タブを使用すると、コンテンツを保存するためのローカルのユニバーサルエディターサービス（`https://localhost:8001/update` または `https://localhost:8001/patch`）へのPOSTリクエストを確認できます。

![ ユニバーサルエディター – WKND チームチームを編集可能 ](./assets/universal-editor-under-the-hood-request.png)

リクエストペイロードの JSON オブジェクトには、コンテンツサーバー（`connections`）、リソースパス（`target`）、更新されたコンテンツ（`patch`）など、必要な詳細が含まれています。

![ ユニバーサルエディター – WKND チームチームを編集可能 ](./assets/universal-editor-under-the-hood-payload.png)

### 編集可能なコンテンツを展開する

編集可能なコンテンツを展開し、実装を **チームメンバー** に適用して、プロパティパネルでチームメンバーを編集できるようにします。

上記のように、関連する `data-aue-*` 属性を `Teams` React コンポーネントのチームメンバーに追加しましょう。

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

   `data-aue-type` 属性の値は、チームメンバーがAEMにコンテンツフラグメントとして保存され `Person` ので `component` であり、コンテンツの移動可能/削除可能な部分を示すのに役立ちます。

1. WKND Teams React アプリを読み込むブラウザーで、ユニバーサルエディターページを更新します。 これで、プロパティパネルを使用してチームメンバーを編集できることが確認できます。

   ![ ユニバーサルエディター – WKND チームチームメンバーが編集可能 ](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### 内部で

上記のように、コンテンツの取得と保存は、ローカルのユニバーサルエディターサービスによって行われます。 `/details`、`/update` または `/patch` のリクエストは、コンテンツの読み込みと保存のためにローカルのユニバーサルエディターサービスに対して行われます。

### コンテンツの追加および削除の定義

これまでのところ、既存のコンテンツを編集可能にしましたが、新しいコンテンツを追加する場合はどうすればよいですか？ ユニバーサルエディターを使用して、WKND チームにチームメンバーを追加または削除する機能を追加しましょう。 したがって、コンテンツ作成者は、AEMに移動してチームメンバーを追加または削除する必要はありません。

ただし、簡単にまとめると、WKND チームメンバーはAEMに `Person` コンテンツフラグメントとして保存され、`teamMembers` プロパティを使用して Team コンテンツフラグメントに関連付けられます。 AEMでモデル定義を確認するには、[my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project) にアクセスしてください。

1. まず、コンポーネント定義ファイル `/public/static/component-definition.json` を作成します。 このファイルには、`Person` コンテンツフラグメントのコンポーネント定義が含まれています。 `aem/cf` プラグインを使用すると、適用するデフォルト値を指定するモデルとテンプレートに基づいて、コンテンツフラグメントを挿入できます。

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

1. 次に、WKND チーム React アプリの `index.html` で、上記のコンポーネント定義ファイルを参照します。 `public/index.html` ファイルの `<head>` セクションを更新して、コンポーネント定義ファイルを含めます。

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

1. 最後に、`src/components/Teams.js` ファイルを更新して、データ属性を追加します。 チームメンバーのコンテナとして機能する **MEMBERS** セクションでは、`data-aue-prop`、`data-aue-type`、`data-aue-label` の属性を `<div>` 要素に追加します。

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

1. WKND Teams React アプリを読み込むブラウザーで、ユニバーサルエディターページを更新します。 **MEMBERS** セクションがコンテナとして機能することがわかります。 新しいチームメンバーを挿入するには、プロパティパネルと「**+**」アイコンを使用します。

   ![ ユニバーサルエディター – WKND チームチームメンバーが挿入 ](./assets/universal-editor-wknd-teams-add-team-members.png)

1. チームメンバーを削除するには、チームメンバーを選択して「**削除**」アイコンをクリックします。

   ![ ユニバーサルエディター – WKND Teams チームメンバーが削除する ](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### 内部で

コンテンツの追加と削除の操作は、ローカルのユニバーサルエディターサービスによって行われます。 詳細なペイロードを含んだ `/add` または `/remove` に対するPOSTリクエストは、AEMにコンテンツを追加または削除するために、ローカルのユニバーサルエディターサービスに対して行われます。

## ソリューションファイル

実装の変更を確認する場合、またはユニバーサルエディターで WKND Teams React アプリを操作できない場合は、[basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) ソリューションブランチを参照してください。

作業中の **basic-tutorial** ブランチとのファイルごとの比較は、（こちら [ で利用でき ](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1) す。

## これで完了です

ユニバーサルエディターを使用してコンテンツを追加、編集、削除するための WKND Teams React アプリのインストルメントが正常に行われました。 コアライブラリを含め、接続とローカルユニバーサルエディターサービスのメタデータを追加し、様々なデータ（`data-aue-*`）属性を使用して React コンポーネントを実装する方法を学びました。
