---
title: AEMコンテンツフラグメントコンソールのヘッダーメニュー拡張機能
description: AEMコンテンツフラグメントコンソールのヘッダーメニュー拡張機能を作成する方法について説明します。
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
source-wordcount: '366'
ht-degree: 0%

---


# ヘッダーメニュー拡張機能

![ヘッダーメニュー拡張機能](./assets/header-menu/header-menu.png){align="center"}

ヘッダーメニューを含む拡張機能。この拡張機能では、AEMコンテンツフラグメントコンソールのヘッダーにボタンが追加されます。このヘッダーは、 __いいえ__ コンテンツフラグメントが選択されている。 ヘッダーメニューの拡張ボタンは、コンテンツフラグメントが選択されていない場合にのみ表示されるので、通常、既存のコンテンツフラグメントには影響しません。 代わりに、ヘッダーメニューの拡張は通常次のようになります。

+ コンテンツフラグメントのセットを作成するなど、コンテンツ参照を介してリンクしたカスタムロジックを使用して、新しいコンテンツフラグメントを作成します。
+ 1 週間以内に作成されたすべてのコンテンツフラグメントを書き出すなど、プログラムによって選択されたコンテンツフラグメントのセットに対して実行する。

## 拡張機能の登録

`ExtensionRegistration.js` はAEM拡張機能のエントリポイントで、次の項目を定義します。

1. 拡張機能のタイプ。ヘッダーメニューボタンの場合。
1. 拡張ボタンの定義 ( `getButton()` 関数に置き換えます。
1. ボタンのクリックハンドラー ( `onClick()` 関数に置き換えます。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## モーダル

![モーダル](./assets/modal/modal.png)

AEMコンテンツフラグメントコンソールのヘッダーメニュー拡張には、次が必要な場合があります。

+ 目的のアクションを実行するためのユーザーからの追加入力。
+ アクションの結果に関するユーザーの詳細情報を提供する機能。

これらの要件をサポートするために、 AEMコンテンツフラグメントコンソール拡張機能を使用すると、React アプリケーションとしてレンダリングするカスタムモーダルを使用できます。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">モーダルの作成にスキップ</p>
      <p class="has-text-blackest">ヘッダーメニュー拡張機能ボタンをクリックすると表示されるモーダルを作成する方法を説明します。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="モーダルの構築について学ぶ">モーダルの構築について学ぶ</span>
        </a>
      </div>
    </div>
  </div>
</div>

## モーダルなし

AEMコンテンツフラグメントコンソールのヘッダーメニュー拡張機能では、次のように、ユーザーとのさらなるやり取りが必要ない場合があります。

+ 読み込みや書き出しなど、ユーザー入力を必要としないバックエンドプロセスを呼び出す。
+ コンテンツのガイドラインに関する内部ドキュメントなど、新しい Web ページを開く。

これらの場合、AEM Content Fragment Console 拡張機能には [モーダル](#modal)を呼び出し、ヘッダーメニューボタンの `onClick` ハンドラ

AEM Content Fragment Console 拡張機能を使用すると、作業の実行中に、進行状況インジケーターでAEMコンテンツフラグメントコンソールをオーバーレイし、ユーザーがそれ以上のアクションを実行できないようにできます。 進行状況インジケータの使用は任意ですが、同期作業の進行状況をユーザに伝えるのに便利です。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
