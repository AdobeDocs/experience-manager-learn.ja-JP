---
title: CSS と JS を使用したブロックの開発
description: ユニバーサルエディターを使用して編集可能な、CSS とJavaScriptを使用したEdge Delivery Services用ブロックを作成します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# CSS とJavaScriptを使用したブロックの開発

[ 前の章 ](./7b-block-js-css.md) では、CSS のみを使用したブロックのスタイル設定について説明しました。 現在は、JavaScriptと CSS の両方を使用したブロックの開発に焦点が移っています。

この例では、次の 3 つの方法でブロックを拡張する方法を示します。

1. カスタム CSS クラスを追加します。
1. イベントリスナーを使用した動きの追加。
1. オプションでティーザーのテキストに含めることができる利用条件の処理。

## 一般的なユースケース

この方法は、次のような状況で特に役立ちます。

- **外部 CSS 管理：** ブロックの CSS がEdge Delivery Servicesの外部で管理され、HTML構造と整合していない場合。
- **追加属性：** アクセシビリティ用の [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) や [ マイクロデータ ](https://developer.mozilla.org/ja/docs/Web/HTML/Microdata) などの追加属性が必要な場合。
- **JavaScriptの機能強化：** イベントリスナーなどのインタラクティブ機能が必要な場合。

この手法はブラウザーネイティブのJavaScript DOM 操作に依存しますが、DOM を変更する際に、特に要素を移動する場合は注意が必要です。 このような変更は、ユニバーサルエディターのオーサリングエクスペリエンスを妨げる可能性があります。 大規模な DOM 変更の必要性を最小限に抑えるために、ブロックの [ コンテンツモデル ](./5-new-block.md#block-model) は慎重に設計されるのが理想です。

## ブロックHTML

ブロック開発にアプローチするには、まず、Edge Delivery Servicesによって公開された DOM を確認します。 JavaScriptで構造を強化し、CSS でスタイルを設定します。

>[!BEGINTABS]

>[!TAB  飾る DOM]

次に、JavaScriptや CSS を使用してデコレートする対象となるティーザーブロックの DOM を示します。

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB DOM を見つける方法 ]

デコレートする DOM を見つけるには、ローカル開発環境でデコレートされていないブロックを含むページを開き、ブロックを選択して、DOM を調べます。

![Inspect ブロック DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## JavaScriptをブロック

ブロックにJavaScript機能を追加するには、ブロックのディレクトリに、ブロックと同じ名前（例：`/blocks/teaser/teaser.js`）でJavaScript ファイルを作成します。

JavaScript ファイルでは、デフォルトの関数を書き出す必要があります。

```javascript
export default function decorate(block) { ... }
```

デフォルトの関数は、Edge Delivery ServicesHTML内のブロックを表す DOM 要素/ツリーを受け取り、ブロックのレンダリング時に実行されるカスタム JavaScriptを含みます。

この例のJavaScriptは、主に次の 3 つのアクションを実行します。

1. CTA ボタンにイベントリスナーを追加し、マウスポインターを置くと画像をズームします。
1. ブロックの要素にセマンティック CSS クラスを追加します。これは、既存の CSS デザインシステムを統合する場合に役立ちます。
1. `Terms and conditions:` で始まる段落に特別な CSS クラスを追加します。

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## CSS をブロック

[ 前の章 ](./7a-block-css.md) で `teaser.css` ーザーを作成した場合は、削除するか、名前を `teaser.css.bak` に変更します。この章では、ティーザーブロックに対して異なる CSS を実装しているからです。

ブロックのフォルダに `teaser.css` ファイルを作成します。 このファイルには、ブロックをスタイル設定する CSS コードが含まれています。 この CSS コードは、ブロックの要素と、`teaser.js` でJavaScriptによって追加された特定のセマンティクス CSS クラスを対象としています。

ベア要素のスタイルは、直接設定することも、カスタムで適用した CSS クラスを使用して設定することもできます。 より複雑なブロックの場合は、セマンティック CSS クラスを適用すると、特に、より長い期間でより大きなチームで作業する場合に、CSS をより理解しやすく維持管理しやすくすることができます。

[ 前と同様に ](./7a-block-css.md#develop-a-block-with-css)、他のブロックとの競合を避けるために、CSS を `.block.teaser` にスコープ設定します。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
}

/* The image is rendered to the first div in the block */
.block.teaser .image-wrapper {
    position: absolute;
    z-index: -1;
    inset: 0;
    box-sizing: border-box;
    overflow: hidden; 
}

.block.teaser .image {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
    transform: scale(1); 
    transition: transform 0.6s ease-in-out;
}

.block.teaser .content {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    background: var(--background-color);
    padding: 1.5rem 1.5rem 1rem;
    width: 80vw;
    max-width: 1200px;
}

.block.teaser .title {
    font-size: var(--heading-font-size-xl);
    margin: 0;
}

.block.teaser .title::after {
    border-bottom: 0;
}

.block.teaser p {
    font-size: var(--body-font-size-s);
    margin-bottom: 1rem;
    animation: teaser-fade-in .6s;
}

.block.teaser p.terms-and-conditions {
    font-size: var(--body-font-size-xs);
    color: var(--secondary-color);
    padding: .5rem 1rem;
    font-style: italic;
    border: solid var(--light-color);
    border-width: 0 0 0 10px;
}

/* Add underlines to links in the text */
.block.teaser a:hover {
    text-decoration: underline;
}

/* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
.block.teaser .button-container {
    margin: 0;
    padding: 0;
}

.block.teaser .button {   
    background-color: var(--primary-color);
    border-radius: 0;
    color: var(--dark-color);
    font-size: var(--body-font-size-xs);
    font-weight: bold;
    padding: 1em 2.5em;
    margin: 0;
    text-transform: uppercase;
}

.block.teaser .zoom {
    transform: scale(1.1);
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## 利用条件を追加

上記の実装により、テキストフ `Terms and conditions:` ールドで始まる特別なスタイル設定を行う段落がサポートされます。 この機能を検証するには、ユニバーサルエディターでティーザーブロックのテキストコンテンツを更新して、利用条件を含めます。

[ ブロックの作成 ](./6-author-block.md) の手順に従い、テキストを編集して最後に **利用条件** 段落を含めます。

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

段落がローカル開発環境で利用条件スタイルでレンダリングされることを確認します。 これらのコードの変更は、ユニバーサルエディターが使用するように設定されている [GitHub のブランチにプッシュされる ](#preview-in-universal-editor) まで、ユニバーサルエディターには反映されません。

## 開発のプレビュー

CSS とJavaScriptが追加されると、AEM CLI のローカル開発環境が変更内容をホットリロードし、コードがブロックに与える影響をすばやく簡単に視覚化できます。 CTAの上にマウスポインターを置き、ティーザーの画像がズームインおよびズームアウトされていることを確認します。

![CSS および JS を使用したティーザーのローカル開発プレビュー ](./assets/7b-block-js-css/local-development-preview.png)

## コードをリンクする

コードの変更を [ 頻繁に lint](./3-local-development-environment.md#linting) して、クリーンで一貫性のある状態に保ちます。 定期的なリンティングは、問題を早期に発見し、開発全体の時間を短縮するのに役立ちます。 リンティングの問題がすべて解決されるまで、開発作業を `main` ブランチに結合することはできません。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## ユニバーサルエディターでプレビュー

AEM ユニバーサルエディターで変更を表示するには、変更を追加してコミットし、ユニバーサルエディターで使用される Git リポジトリーブランチにプッシュします。 これにより、ブロックを実装してもオーサリングエクスペリエンスが妨げられることはありません。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

これで、`?ref=teaser` クエリパラメーターを追加する際に、ユニバーサルエディターで変更をプレビューできます。

![ ユニバーサルエディターのティーザー ](./assets/7b-block-js-css/universal-editor-preview.png)

