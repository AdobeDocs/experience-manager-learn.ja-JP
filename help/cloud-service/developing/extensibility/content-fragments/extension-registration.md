---
title: AEMコンテンツフラグメントコンソール拡張機能の登録
description: コンテンツフラグメントコンソール拡張機能の登録方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# 拡張機能の登録

AEMコンテンツフラグメントコンソールの拡張は、React に基づく特殊な App Builder アプリで、 [React スペクトル](https://react-spectrum.adobe.com/react-spectrum/) UI フレームワーク。

拡張機能のAEM Content Fragment Console を表示する場所と方法を定義するには、拡張機能の App Builder アプリケーションで次の 2 つの特定の設定が必要です。アプリのルーティングと拡張機能の登録。

## アプリルート{#app-routes}

拡張機能の `App.js` 宣言する [React ルーター](https://reactrouter.com/en/main) AEMコンテンツフラグメントコンソールに拡張機能を登録するインデックスルートを含む

AEM Content Fragment Console が最初に読み込まれると、インデックスルートが呼び出され、このルートのターゲットによって、拡張機能がコンソールに公開される方法が定義されます。

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

`ExtensionRegistration.js` は、拡張機能のインデックスルートを介して直ちに読み込まれ、次を定義して、拡張機能の登録ポイントを操作します。

1. 拡張機能のタイプ。a [ヘッダーメニュー](./header-menu.md) または [アクションバー](./action-bar.md) 」ボタンをクリックします。
   + [ヘッダーメニュー](./header-menu.md) 拡張子は、 `headerMenu` プロパティ： `methods`.
   + [アクションバー](./action-bar.md) 拡張子は、 `actionBar` プロパティ： `methods`.
1. 拡張ボタンの定義 ( `getButton()` 関数に置き換えます。 この関数は、次のフィールドを持つオブジェクトを返します。
   + `id` は、ボタンの一意の ID です
   + `label` は、AEMコンテンツフラグメントコンソールの拡張機能ボタンのラベルです。
   + `icon` は、AEMコンテンツフラグメントコンソールの拡張機能ボタンのアイコンです。 アイコンは、 [React スペクトル](https://spectrum.adobe.com/page/icons/) アイコン名（スペースを削除）。
1. ボタンのクリックハンドラー ( 内で `onClick()` 関数に置き換えます。
   + [ヘッダーメニュー](./header-menu.md) 拡張機能は、クリックハンドラーにパラメーターを渡しません。
   + [アクションバー](./action-bar.md) 拡張機能を使用すると、選択したコンテンツフラグメントのパスのリストが `selections` パラメーター。

### ヘッダーメニュー拡張機能

![ヘッダーメニュー拡張機能](./assets/extension-registration/header-menu.png)

コンテンツフラグメントが選択されていない場合は、ヘッダーメニュー拡張ボタンが表示されます。 ヘッダーメニュー拡張機能はコンテンツフラグメントの選択には影響しないので、コンテンツフラグメントにはコンテンツフラグメントは提供されません `onClick()` ハンドラ

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">ヘッダーメニュー拡張機能の作成にスキップします</p>
      <p class="has-text-blackest">AEMコンテンツフラグメントコンソールでヘッダーメニュー拡張機能を登録および定義する方法について説明します。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="ヘッダーメニュー拡張機能の作成について説明します">ヘッダーメニュー拡張機能の作成について説明します</span>
        </a>
      </div>
    </div>
  </div>
</div>

### アクションバー拡張機能

![アクションバー拡張機能](./assets/extension-registration/action-bar.png)

1 つ以上のコンテンツフラグメントが選択されている場合は、アクションバーの拡張ボタンが表示されます。 選択したコンテンツフラグメントのパスは、 `selections` パラメーター、ボタンの `onClick(..)` ハンドラ

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">アクションバー拡張機能の作成にスキップします</p>
      <p class="has-text-blackest">AEMコンテンツフラグメントコンソールでアクションバー拡張機能を登録および定義する方法について説明します。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="アクションバー拡張機能の構築について説明します">アクションバー拡張機能の構築について説明します</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 条件付きで拡張機能を含める

AEMコンテンツフラグメントコンソール拡張機能は、カスタムロジックを実行して、拡張機能がAEMコンテンツフラグメントコンソールに表示されるタイミングを制限できます。 このチェックは、 `register` を呼び出す `ExtensionRegistration` コンポーネントに含まれ、拡張機能を表示しない場合は直ちに返されます。

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
