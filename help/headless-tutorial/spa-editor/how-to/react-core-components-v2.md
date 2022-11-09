---
title: AEM React 編集可能コンポーネント v2 の使用方法
description: AEM React 編集可能コンポーネント v2 を使用して React アプリを強化する方法を説明します。
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---


# AEM React 編集可能コンポーネント v2 の使用方法

AEM提供 [AEM React 編集可能コンポーネント v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components):AEM SPA Editor を使用したコンテキスト内コンポーネントの編集をサポートする、React コンポーネントの作成を可能にする Node.js ベースの SDK。

+ [npm モジュール](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github プロジェクト](https://github.com/adobe/aem-react-editable-components)
+ [Adobe文書](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


AEM React Editable Components v2 の詳細とコードサンプルについては、技術ドキュメントを参照してください。

+ [AEMドキュメントとの統合](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [編集可能なコンポーネントのドキュメント](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [ヘルパードキュメント](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEMページ

AEM React 編集可能コンポーネントは、SPA Editor またはリモートSPA React アプリの両方で動作します。 編集可能な React コンポーネントを生成するコンテンツは、 [SPA Page コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). 編集可能な React コンポーネントにマッピングするAEMコンポーネントは、AEMを実装する必要があります。 [コンポーネントエクスポーターフレームワーク](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=ja)  — 例： [AEM Core WCM Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja).


## 依存関係

React アプリが Node.js 14 以降で実行されていることを確認します。

AEM React Editable Components v2 を使用する React アプリの依存関係は、以下のとおりです。 `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`、および  `@adobe/aem-spa-page-model-manager`.


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

## SPA Editor

SPAエディターベースの React アプリでAEM React 編集可能コンポーネントを使用する場合、AEM `ModelManager` SDK、SDK として：

1. AEMからコンテンツを取得します
1. React Edible コンポーネントにAEMコンテンツを入力します

初期化された ModelManager で React アプリをラップし、React アプリをレンダリングします。 React アプリには、 `<Page>` から書き出されたコンポーネント `@adobe/aem-react-editable-components`. この `<Page>` コンポーネントには、 `.model.json` AEMが提供

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

この `<Page>` は、を介してAEMページの表現を JSON として渡します。 `pageModel` 提供元： `ModelManager`. この `<Page>` コンポーネントは、内のオブジェクトに対して React コンポーネントを動的に作成します。 `pageModel` 一致する `resourceType` を介して自身をリソースタイプに登録する React コンポーネントとの統合 `MapTo(..)`.

## 編集可能なコンポーネント

この `<Page>` は、AEMページの表現を JSON として、 `ModelManager`. この `<Page>` 次に、JSON 内の各オブジェクトに対して、JS オブジェクトの `resourceType` 値を React コンポーネントと共に生成し、その React コンポーネントが、コンポーネントの `MapTo(..)` 呼び出し。 例えば、次の例を使用して、インスタンスをインスタンス化します

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

AEMが提供する上記の JSON は、編集可能な React コンポーネントを動的にインスタンス化し、設定するために使用できます。

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

編集可能なコンポーネントは、相互に再利用および埋め込むことができます。 編集可能なコンポーネントを別のコンポーネントに埋め込む際には、次の 2 つの重要な考慮事項があります。

1. 埋め込みコンポーネントを満たすには、AEMの JSON コンテンツにコンテンツが含まれている必要があります。 これをおこなうには、必要なデータを収集するAEMコンポーネントのダイアログを作成します。
1. React コンポーネントの「編集不可」インスタンスは、でラップされた「編集可能」インスタンスではなく、埋め込む必要があります。 `<EditableComponent>`. これは、埋め込みコンポーネントが `<EditableComponent>` ラッパーを使用する場合、SPA Editor は、外側の埋め込みコンポーネントではなく、編集クロム（青いホバーボックス）を使用して内側のコンポーネントをドレス化しようとします。

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

AEMが提供する上記の JSON は、別の React コンポーネントを埋め込んだ編集可能な React コンポーネントを動的にインスタンス化し、設定するために使用できます。


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



