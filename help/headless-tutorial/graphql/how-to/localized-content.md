---
title: AEMヘッドレスでのローカライズされたコンテンツの使用
description: GraphQLを使用して、ローカライズされたコンテンツをAEMに問い合わせる方法を説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---


# AEMヘッドレスを使用したコンテンツのローカライズ

AEMが [翻訳統合フレームワーク](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) ヘッドレスコンテンツの場合、コンテンツフラグメントやサポートアセットをロケール間で容易に翻訳して使用できます。 これは、ページ、エクスペリエンスフラグメント、アセット、Formsなど、他のAEMコンテンツを翻訳する場合と同じフレームワークです。 1 回 [ヘッドレスコンテンツが翻訳されました](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=ja)、および公開された場合、ヘッドレスアプリケーションで使用する準備が整います。

## アセットフォルダー構造{#assets-folder-structure}

AEMでローカライズされたコンテンツフラグメントが [推奨位置構造](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![AEM assets フォルダーのローカライズ](./assets/localized-content/asset-folders.jpg)

ロケールフォルダーは兄弟である必要があり、フォルダー名は title ではなく、有効な名前である必要があります [ISO 639-1 コード](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) フォルダーに含まれるコンテンツのロケールを表します。

ロケールコードは、GraphQLクエリによって返されるコンテンツフラグメントのフィルタリングにも使用される値です。

| ロケールコード | AEM path | コンテンツのロケール |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | ドイツ語コンテンツ |
| en | /content/dam/.../**en**/... | 英語コンテンツ |
| es | /content/dam/.../**es**/... | スペイン語コンテンツ |

## GraphQL永続クエリ

AEMが `_locale` ロケールコードでコンテンツを自動的にフィルタリングするGraphQLフィルター。 例えば、 [WKND Site プロジェクト](https://github.com/adobe/aem-guides-wknd) は、新しい永続クエリで実行できます `wknd-shared/adventures-by-locale` 次のように定義されます。

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

この `$locale` 変数 `_locale` フィルタにはロケールコードが必要です ( 例： `en`, `en_us`または `de`) を [AEMアセットフォルダーベースのローカリゼーション規則](#assets-folder-structure).

## React の例

ロケールセレクターを使用し、ロケールセレクターに基づいてAEMからクエリする Adventure コンテンツを制御する、シンプルな React アプリケーションを作成します。 `_locale` フィルター。

条件 __英語__ ロケールセレクターでが選択され、次に英語のアドベンチャーコンテンツフラグメントが `/content/dam/wknd/en` が返される場合、 __スペイン語__ が選択され、次に下にスペイン語コンテンツフラグメントが選択されます。 `/content/dam/wknd/es`など。

![ローカライズされた React サンプルアプリ](./assets/localized-content/react-example.png)

### の作成 `LocaleContext`{#locale-context}

まず、 [React コンテキスト](https://reactjs.org/docs/context.html) を使用して、React アプリケーションのコンポーネント全体でロケールを使用できるようにします。

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### の作成 `LocaleSwitcher` React コンポーネント{#locale-switcher}

次に、 [LocaleContext の](#locale-context) の値をユーザーの選択範囲に追加します。

このロケール値は、GraphQLクエリを実行し、選択したロケールに一致するコンテンツのみを返すようにするために使用されます。

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### を使用してコンテンツをクエリする `_locale` フィルター{#adventures}

Adventures コンポーネントは、ロケール別にすべての冒険をAEMに問い合わせ、そのタイトルを表示します。 これを実現するには、React コンテキストに格納されたロケール値を `_locale` フィルター。

この方法は、アプリケーション内の他のクエリに拡張でき、すべてのクエリにユーザーのロケール選択で指定された内容のみが含まれるようにします。

AEMに対するクエリは、カスタム React フックで実行されます。 [getAdventuresByLocale(AEM GraphQLのクエリに関するドキュメントで詳しく説明 )](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### 次を定義： `App.js`{#app-js}

最後に、React アプリケーションを `LanguageContext.Provider` ロケール値の設定 これにより、他の React コンポーネント [LocaleSwitcher](#locale-switcher)、および [冒険](#adventures) ロケール選択の状態を共有する。

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
