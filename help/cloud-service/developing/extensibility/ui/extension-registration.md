---
title: AEM UI 拡張機能の登録
description: AEM UI 拡張機能の登録方法について説明します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '299'
ht-degree: 100%

---

# 拡張機能の登録

AEM UI 拡張機能は、React をベースとする特殊な Application Builder アプリで、[React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) UI フレームワークを使用します。

AEM UI 拡張機能が表示される場所と方法を定義するには、拡張機能の Application Builder アプリで、アプリのルーティングと拡張機能の登録という 2 つの設定が必要です。

## アプリルート{#app-routes}

拡張機能の `App.js` では、AEM UI に拡張機能を登録するインデックスルートを含む [React ルーター](https://reactrouter.com/ja/main)を宣言します。

AEM UI が最初に読み込まれると、インデックスルートが呼び出されます。このルートのターゲットは、拡張機能がコンソールでどのように公開されるかを定義します。

+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 拡張機能の登録

`ExtensionRegistration.js` は、拡張機能のインデックスルートを通じて直ちに読み込まれる必要があり、拡張機能の登録ポイントとして機能します。

[Application Builder アプリ拡張機能を初期化](./app-initialization.md)する際に選択した AEM UI 拡張機能テンプレートに応じて、様々な拡張ポイントがサポートされています。

+ [コンテンツフラグメント UI 拡張ポイント](./content-fragments/overview.md#extension-points)

## 拡張機能の条件付き組み込み

AEM UI 拡張機能では、カスタムロジックを実行して、拡張機能が表示される AEM 環境を制限できます。このチェックは `ExtensionRegistration` コンポーネントでの `register` 呼び出しの前に実行され、拡張機能を表示すべきでない場合は直ちに戻ります。

このチェックで使用できるコンテキストは限られています。

+ 拡張機能が読み込まれる AEM ホスト。
+ 現在のユーザーの AEM アクセストークン。

拡張機能を読み込むための最も一般的なチェックは、次のとおりです。

+ AEMホスト（`new URLSearchParams(window.location.search).get('repo')`）を使用して、拡張機能を読み込むかどうかを判断します。
   + 特定のプログラムの構成要素である AEM 環境でのみ拡張機能を表示します（以下の例を参照）。
   + 特定の AEM 環境（AEM ホスト）でのみ拡張機能を表示します。
+ [Adobe I/O Runtime アクション](./runtime-action.md)を使用して AEM への HTTP 呼び出しを行い、現在のユーザーに拡張機能を表示するかどうかを判断します。

次の例は、プログラム `p12345` のすべての環境に拡張機能を制限する方法を示しています。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
