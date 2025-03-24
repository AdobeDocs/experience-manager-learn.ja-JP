---
title: AEM ヘッドレスとローカライズされたコンテンツの使用
description: GraphQL を使用して、ローカライズされたコンテンツを AEM にクエリする方法について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 100%

---

# AEM ヘッドレスとローカライズされたコンテンツ

AEM は、ヘッドレスコンテンツに[翻訳統合フレームワーク](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=ja)を提供しており、コンテンツのフラグメントとサポートアセットを、ロケール間で容易に翻訳して使用できます。このフレームワークは、ページ、エクスペリエンスフラグメント、アセット、フォームなどの、その他の AEM コンテンツの翻訳に使用するフレームワークと同じです。[ヘッドレスコンテンツを翻訳して](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=ja)、公開すると、ヘッドレスアプリケーションで使用する準備が整います。

## アセットフォルダー構造{#assets-folder-structure}

AEM のローカライズされたコンテンツフラグメントが、[推奨のローカライゼーション構造](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure)になっていることを確認します。

![ローカライズされた AEM アセットフォルダー](./assets/localized-content/asset-folders.jpg)

ロケールフォルダーは兄弟である必要があり、フォルダー名はタイトルではなく、フォルダーに含まれるコンテンツのロケールを表す有効な [ISO 639-1 コード](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)でなければなりません。

ロケールコードは、GraphQL クエリによって返されるコンテンツフラグメントのフィルタリングにも使用される値です。

| ロケールコード | AEM パス | コンテンツのロケール |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | ドイツ語コンテンツ |
| en | /content/dam/.../**en**/... | 英語コンテンツ |
| es | /content/dam/.../**es**/... | スペイン語コンテンツ |

## GraphQL 永続クエリ

AEM は、ロケールコードごとにコンテンツを自動的にフィルタリングする `_locale` GraphQL フィルターを提供します。例えば、[WKND Site プロジェクト](https://github.com/adobe/aem-guides-wknd)のすべての英語のアドベンチャーのクエリは、次のように定義された新しい永続クエリ `wknd-shared/adventures-by-locale` で実行できます。

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

`_locale` フィルターで使用される `$locale` 変数には、[AEM のアセットフォルダーベースのローカライゼーション規則](#assets-folder-structure)で指定されているロケールコード（`en`、`en_us`、`de`など）が必要です。

## React の例

`_locale` フィルターを使用して、ロケールセレクターに基づき、AEM からクエリするアドベンチャーコンテンツを制御できるシンプルな React アプリケーションを作成します。

例えば、ロケールセレクターで&#x200B;__英語__&#x200B;を選択すると `/content/dam/wknd/en` の下にある英語のアドベンチャーのコンテンツフラグメントが返され、__スペイン語__&#x200B;を選択すると `/content/dam/wknd/es` の下にあるスペイン語のコンテンツフラグメントが返されます。

![ローカライズされた React アプリの例](./assets/localized-content/react-example.png)

### `LocaleContext` の作成{#locale-context}

まず、[React コンテキスト](https://reactjs.org/docs/context.html)を作成して、React アプリケーションのコンポーネント全体でロケールを使用できるようにします。

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

### `LocaleSwitcher` React コンポーネントの作成{#locale-switcher}

次に、ユーザーの選択範囲に [LocaleContext の ](#locale-context) 値を設定するロケールスイッチャー React コンポーネントを作成します。

このロケール値を使用すると、GraphQL クエリを実行し、選択したロケールに一致するコンテンツのみを返すことができます。

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

### `_locale` フィルターを使用したコンテンツのクエリ{#adventures}

Adventures コンポーネントは、ロケール別のすべてのアドベンチャーに関して AEM にクエリを実行し、タイトルを一覧表示します。これを実現するには、`_locale` フィルターを使用して React コンテキストに保存されているロケール値をクエリに渡します。

この方法はアプリケーション内の他のクエリに拡張でき、すべてのクエリにユーザーのロケール選択で指定されたコンテンツのみを含むようにすることができます。

AEM に対するクエリは、[「AEM GraphQL のクエリ」ドキュメントで詳細に説明されているカスタム React フック getAdventuresbyLocale](./aem-headless-sdk.md) で実行されます。

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

### `App.js` の定義{#app-js}

最後に、React アプリケーションを `LanguageContext.Provider` にラップして、ロケール値を設定し、すべてを結合します。これにより、他の React コンポーネント、[ロケールスイッチャー](#locale-switcher)、[アドベンチャー](#adventures)でロケール選択状態を共有できます。

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
