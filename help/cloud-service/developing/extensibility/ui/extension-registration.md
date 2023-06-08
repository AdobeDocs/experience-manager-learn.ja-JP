---
title: AEM UI 拡張機能の登録
description: AEM UI 拡張機能の登録方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 4%

---

# 拡張機能の登録

AEM UI 拡張機能は、React に基づく特殊な App Builder アプリで、 [React スペクトル](https://react-spectrum.adobe.com/react-spectrum/) UI フレームワーク。

AEM UI 拡張機能が表示される場所と方法を定義するには、拡張機能の App Builder アプリケーションで次の 2 つの設定が必要です。アプリのルーティングと拡張機能の登録。

## アプリルート{#app-routes}

拡張機能の `App.js` 宣言する [React ルーター](https://reactrouter.com/ja/main) AEM UI で拡張を登録するインデックスルートを含む

AEM UI が最初に読み込まれるとインデックスルートが呼び出され、このルートのターゲットは拡張機能がコンソールに公開される方法を定義します。

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

`ExtensionRegistration.js` は、拡張機能のインデックスルートを介して直ちに読み込まれ、拡張機能の登録ポイントを操作する必要があります。

次の場合に選択したAEM UI 拡張機能テンプレートに基づく [App Builder アプリ拡張機能の初期化](./app-initialization.md)の場合、異なる拡張ポイントがサポートされます。

+ [コンテンツフラグメント UI 拡張ポイント](./content-fragments/overview.md#extension-points)


## 条件付きで拡張機能を含める

AEM UI 拡張機能は、カスタムロジックを実行して、拡張機能が表示されるAEM環境を制限できます。 このチェックは、 `register` を呼び出す `ExtensionRegistration` コンポーネントに含まれ、拡張機能を表示しない場合は直ちに返されます。

このチェックには使用可能なコンテキストが制限されています：

+ 拡張機能が読み込まれるAEMホスト。
+ 現在のユーザーのAEMアクセストークン。

拡張機能の読み込みに関する最も一般的なチェックは次のとおりです。

+ AEMホスト (`new URLSearchParams(window.location.search).get('repo')`) を使用して、拡張機能を読み込む必要があるかどうかを判断します。
   + 特定のプログラムの一部であるAEM環境でのみ拡張機能を表示します（以下の例を参照）。
   + 特定のAEM環境 (AEMホスト ) でのみ拡張機能を表示します。
+ の使用 [Adobe I/O Runtime action](./runtime-action.md) を使用してAEMに対する HTTP 呼び出しをおこない、現在のユーザーに拡張機能を表示する必要があるかどうかを判断します。

次の例は、プログラム内のすべての環境に拡張機能を制限する方法を示しています `p12345`.

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
