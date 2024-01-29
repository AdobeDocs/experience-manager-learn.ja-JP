---
title: AEM React 編集可能コンポーネント v2 の使用方法
description: AEM React 編集可能コンポーネント v2 を使用して React アプリを強化する方法を説明します。
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 195
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '525'
ht-degree: 100%

---

# AEM React 編集可能コンポーネント v2 の使用方法

{{edge-delivery-services}}

AEM は [AEM React 編集可能コンポーネント v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components) を提供しています。これは、React コンポーネントの作成を可能にする Node.js ベースの SDK であり、AEM SPA エディターを使用したコンテキスト内コンポーネント編集をサポートしています。

+ [npm モジュール](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [GitHub プロジェクト](https://github.com/adobe/aem-react-editable-components)
+ [アドビのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html?lang=ja)


AEM React 編集可能コンポーネント v2 の詳細とコードサンプルについては、技術ドキュメントを参照してください。

+ [AEM ドキュメントとの統合](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [編集可能コンポーネントのドキュメント](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [ヘルパーのドキュメント](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM ページ

AEM React 編集可能コンポーネントは、SPA エディターとリモート SPA React アプリの両方で動作します。 React 編集可能コンポーネントに入力するコンテンツは、[SPA ページコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html?lang=ja)を拡張した AEM ページを通じて公開する必要があります。編集可能な React コンポーネントにマッピングされる AEM コンポーネントは、AEM の [コンポーネントエクスポーターフレームワーク](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=ja)（[AEM WCM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)など）を実装する必要があります。


## 依存関係

React アプリが Node.js 14 以降で動作していることを確認します。

React アプリで AEM React 編集可能コンポーネント v2 を使用するための依存関係の最小限のセットは、`@adobe/aem-react-editable-components`、`@adobe/aem-spa-component-mapping` および `@adobe/aem-spa-page-model-manager` です。


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base) および [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) は、AEM React 編集可能コンポーネント v2 と互換性がありません。

## SPA エディター

AEM React 編集可能コンポーネント を SPA エディターベースの React アプリで使用する場合、AEM `ModelManager` SDK は SDK として次の機能を果たします。

1. AEM からコンテンツを取得します。
1. React 編集可能コンポーネントに AEM のコンテンツを入力します。

初期化された ModelManager で React アプリをラップし、React アプリをレンダリングします。 React アプリには、`@adobe/aem-react-editable-components` から書き出された `<Page>` コンポーネントのインスタンスが 1 つ含まれている必要があります。`<Page>` コンポーネントには、AEM から提供される `.model.json` に基づいて React コンポーネントを動的に作成するロジックがあります。

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

`<Page>` には、`ModelManager` から提供される `pageModel` を通じて、AEM ページの表現が JSON として渡されます。`<Page>` コンポーネントは、`resourceType` を、`MapTo(..)` を通じてリソースタイプに自分自身を登録する React コンポーネントと照合することにより、`pageModel` 内のオブジェクトの React コンポーネントを動的に作成します。

## 編集可能コンポーネント

`<Page>` には、`ModelManager` を通じて AEM ページの表現が JSON として渡されます。次に `<Page>` コンポーネントは、JS オブジェクトの `resourceType` 値を、コンポーネントの `MapTo(..)` 呼び出しを通じてリソースタイプに自分自身を登録する React コンポーネントと照合することにより、JSON 内のオブジェクトごとに React コンポーネントを動的に作成します。例えば、以下のようにして、インスタンスをインスタンス化します。

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

AEM から提供される上記の JSON を使用すると、編集可能な React コンポーネントを動的にインスタンス化し、情報を入力することができます。

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## コンポーネントの埋め込み

編集可能コンポーネントは再利用でき、相互に埋め込むことができます。 一方の編集可能コンポーネントをもう一方の編集可能コンポーネントに埋め込む際は、考慮すべき重要な事項が次の 2 つあります。

1. 埋め込む側のコンポーネントの JSON コンテンツ（AEM 内）には、埋め込まれる側のコンポーネントを満たすコンテンツが含まれている必要があります。これを実現するには、必要なデータを収集する AEM コンポーネントのダイアログを作成します。
1. `<EditableComponent>` でラップされた「編集可能」インスタンスではなく、React コンポーネントの「編集不可」インスタンスを埋め込む必要があります。これは、埋め込みコンポーネントに `<EditableComponent>` ラッパーがある場合、SPA エディターは、外側の埋め込みコンポーネントではなく、内側のコンポーネントを編集クロム（青いホバーボックス）で装飾しようとするためです。

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

AEM が提供する上記の JSON は、別の React コンポーネントを埋め込んだ編集可能 React コンポーネントを動的にインスタンス化し、設定するために使用できます。


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
