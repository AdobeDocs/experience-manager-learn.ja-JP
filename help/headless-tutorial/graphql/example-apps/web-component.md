---
title: Web コンポーネント/JS - AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Web コンポーネント/JS アプリケーションは、永続化されたクエリを使用してAEM GraphQL API を使用してコンテンツをクエリする方法を示します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 7%

---


# Web コンポーネント

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 この Web コンポーネントアプリケーションでは、永続的なクエリを使用してAEM GraphQL API を使用してコンテンツをクエリし、純粋な JavaScript コードを使用して UI の一部をレンダリングする方法を示します。

![AEMヘッドレスを備えた Web コンポーネント](./assets/web-component/web-component.png)

次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) ( ローカルのAEM 6.5 またはAEM SDK に接続する場合 )
+ [Node.js v10 以降](https://nodejs.org/ja/)
+ [npm 6 以降](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要件

Web コンポーネントは、次のAEMデプロイメントオプションと連携します。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances)

すべてのデプロイメントには `tutorial-solution-content.zip` から [ソリューションファイル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 設置し、必要とする [デプロイメント設定](../deployment/web-component.md) が実行されます。


>[!IMPORTANT]
>
>Web コンポーネントは、 __AEM パブリッシュ__ 環境で使用できますが、Web コンポーネントの [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) ファイル。

## 使用方法

1. のクローン `adobe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. に移動します。 `web-component` サブディレクトリ。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. を編集します。 `.../src/person.js` AEM接続の詳細を含めるファイル：

   内 `aemHeadlessService` オブジェクト、 `aemHost` をクリックして、AEM パブリッシュサービスを指すように設定します。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   AEM オーサーサービスに接続している場合は、 `aemCredentials` オブジェクトです。ローカルのAEMユーザー資格情報を入力します。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. ターミナルを開き、次のコマンドを実行します。 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウが開き、Web コンポーネントを埋め込んだ静的HTMLページが次の場所に埋め込まれます。 [http://localhost:8080](http://localhost:8080).
1. この _担当者情報_ Web コンポーネントが Web ページに表示されます。

## コード

Web コンポーネントの構築方法、GraphQL の永続クエリを使用してコンテンツを取得するAEMヘッドレスへの接続方法、そのデータの表示方法の概要を次に示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Web コンポーネントHTMLタグ

再利用可能な Web コンポーネント（カスタム要素） `<person-info>` が `../src/assets/aem-headless.html` HTMLページ。 対応 `host` および `query-param-value` 属性を使用して、コンポーネントの動作を制御します。 この `host` 属性の値のオーバーライド `aemHost` 値 `aemHeadlessService` オブジェクト `person.js`、および `query-param-value` を使用して、レンダリングするユーザーを選択します。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web コンポーネントの実装

この `person.js` は Web コンポーネント機能を定義しています。主な特徴は次のとおりです。

#### PersonInfo 要素の実装

この `<person-info>` カスタム要素のクラスオブジェクトは、 `connectedCallback()` ライフサイクルメソッド、シャドウルートの添付、GraphQL の永続クエリの取得、カスタム要素の内部シャドウ DOM 構造を作成する DOM 操作。

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### を登録します。 `<person-info>` 要素

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### クロスオリジンリソース共有 (CORS)

この Web コンポーネントは、ターゲットAEM環境で実行されるAEMベースの CORS 設定に依存し、ホストページがで実行されることを前提としています `http://localhost:8080` 開発モードでは、以下に、ローカル AEM オーサーサービスのサンプル CORS OSGi 設定を示します。

確認してください [デプロイメント設定](../deployment/web-component.md) 各AEMサービスの

![CORS 設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
