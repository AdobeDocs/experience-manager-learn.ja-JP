---
title: AEM コンテンツフラグメントコンソールのアクションバーの拡張
description: AEM コンテンツフラグメントコンソールのアクションバー拡張機能を作成する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: ht
source-wordcount: '333'
ht-degree: 100%

---


# アクションバー拡張機能

![アクションバーの拡張機能](./assets/action-bar/action-bar.png){align="center"}

アクションバーを含む拡張機能では、AEM コンテンツフラグメントコンソールのアクションに、__1 つ以上__&#x200B;のコンテンツフラグメントが選択されたときに表示されるボタンを導入します。アクションバーの拡張ボタンは、少なくとも 1 つのコンテンツフラグメントが選択されたときにのみ表示されるため、通常は選択されたコンテンツフラグメントに対して動作します。以下に例を示します。

+ 選択したコンテンツフラグメントでのビジネスプロセスまたはワークフローの呼び出し。
+ 選択したコンテンツフラグメントのデータを更新または変更します。

## 拡張機能の登録

`ExtensionRegistration.js` は AEM 拡張機能のエントリポイントで、次の項目を定義します。

1. 拡張機能の種類。この場合はアクションバーボタン。
1. `getButton()` 関数内の、拡張機能ボタンの定義。
1. `onClick()` 関数内のボタンのクリックハンドラー。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## モーダル

![モーダル](./assets/modal/modal.png)

AEM コンテンツフラグメントコンソールのアクションバーの拡張機能には、次が必要な場合があります。

+ 目的のアクションを実行するためのユーザーからの追加入力。
+ アクションの結果に関するユーザーの詳細情報を提供する機能。

これらの要件をサポートするために、AEM コンテンツフラグメントコンソールの拡張機能を使用すると、React アプリケーションとしてレンダリングするカスタムモーダルを使用できます。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">モーダルの作成にスキップ</p>
      <p class="has-text-blackest">アクションバーの拡張機能ボタンをクリックしたときに表示されるモーダルの作成方法を説明します。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="モーダルの構築について学ぶ">モーダルの構築について学ぶ</span>
        </a>
      </div>
    </div>
  </div>
</div>

## モーダルなし

AEM コンテンツフラグメントコンソールのアクションバー拡張機能では、次のように、ユーザーとのさらなるやり取りが必要ない場合があります。

+ 読み込みや書き出しなど、ユーザー入力を必要としないバックエンドプロセスを呼び出す。

このような場合、AEM コンテンツフラグメントコンソールの拡張機能は[モーダル](#modal)を必要とせず、アクションバーボタンの `onClick` ハンドラーで直接作業を実行します。

AEM コンテンツフラグメントコンソール拡張機能を使用すると、作業中に進行状況インジケーターで AEM コンテンツフラグメントコンソールをオーバーレイし、ユーザーがそれ以上のアクションを実行するのを防ぐことができます。進行状況インジケーターの使用は任意ですが、同期作業の進行状況をユーザーに伝えるのに便利です。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
