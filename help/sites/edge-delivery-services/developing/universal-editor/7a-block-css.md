---
title: CSS を使用したブロックの開発
description: ユニバーサルエディターを使用して編集できる、Edge Delivery Services 用の CSS を使用してブロックを開発します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 14cda9d4-752b-4425-a469-8b6f283ce1db
source-git-commit: 2722a4d4a34172e2f418f571f9de3872872e682a
workflow-type: ht
source-wordcount: '437'
ht-degree: 100%

---

# CSS を使用したブロックの開発

Edge Delivery Services のブロックは、CSS を使用してスタイル設定されます。ブロックの CSS ファイルは、ブロックのディレクトリに保存され、ブロックと同じ名前が付けられます。例えば、`teaser` という名前のブロックの CSS ファイルは、`blocks/teaser/teaser.css` にあります。

理想的には、ブロックでは、DOM を変更したり CSS クラスを追加したりするために JavaScript に依存することなく、スタイル設定に CSS のみが必要になります。JavaScript の必要性は、ブロックの[コンテンツモデリング](./5-new-block.md#block-model)とその複雑さによって異なります。必要に応じて、[ブロックの JavaScript](./7b-block-js-css.md) を追加できます。

CSS のみのアプローチを使用すると、ブロックの（ほとんどの）ベアセマンティック HTML 要素がターゲットとなり、スタイル設定されます。

## ブロックの HTML

ブロックのスタイル設定方法を理解するには、まず、スタイル設定に使用できる Edge Delivery Services によって公開される DOM を確認します。DOM は、AEM CLI のローカル開発環境によって提供されるブロックを調べることで確認できます。ユニバーサルエディターの DOM は若干異なるので、使用しないでください。

>[!BEGINTABS]

>[!TAB スタイル設定する DOM]

以下は、スタイル設定のターゲットとなるティーザーブロックの DOM です。

`<p class="button-container">...` は、Edge Delivery Services JavaScript によって推測された要素として[自動的に拡張](./4-website-branding.md#inferred-elements)されます。

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

>[!TAB DOM を見つける方法]

スタイル設定する DOM を見つけるには、ローカル開発環境でスタイル設定されていないブロックを含むページを開き、ブロックを選択して DOM を検査します。

![ブロックの DOM の検査](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## ブロックの CSS

ブロックの名前をファイル名として使用して、ブロックのフォルダーに新しい CSS ファイルを作成します。例えば、**ティーザー**&#x200B;ブロックの場合、ファイルは `/blocks/teaser/teaser.css` にあります。

この CSS ファイルは、Edge Delivery Services の JavaScript がページ上でティーザーブロックを表す DOM 要素を検出すると自動的に読み込まれます。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` using CSS nesting (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The image is rendered to the first div in the block */
    picture {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;

        img {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
        }
    }

    /** 
    The teaser's text is rendered to the second (also the last) div in the block.

    These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

    This div order can be used to target different styles to the same semantic elements in the block. 
    For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
    and the second image with `.block.teaser > div:nth-child(2) img`.
    **/
    & > div:last-child {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;

        /** 
        The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

        .block.teaser > div:last-child p { .. }

        However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
        **/

        /* Regardless of the authored heading level, we only want one style the heading */
        h1,
        h2,
        h3,
        h4,
        h5,
        h6 {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        h1::after,
        h2::after,
        h3::after,
        h4::after,
        h5::after,
        h6::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;

            .button {
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
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

## 開発プレビュー

CSS はコードプロジェクトに書き込まれているので、AEM CLI のホットリロードによって変更が行われ、CSS がブロックにブロックに与える影響をすばやく簡単に確認できます。

![CSS のみのプレビュー](./assets/7a-block-css/local-development-preview.png)

## コードのリント

コードの変更がクリーンで一貫性のあることを確認するために、[頻繁にリント](./3-local-development-environment.md#linting)します。リンティングを行うと、問題を早期に発見し、全体的な開発時間を短縮できます。すべてのリンティングの問題が解決されるまで、開発作業を `main` に結合できません。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## ユニバーサルエディターでのプレビュー

AEM のユニバーサルエディターで変更を表示するには、ユニバーサルエディターで使用される Git リポジトリ分岐に変更を追加、コミット、プッシュします。この手順は、ブロックの実装によってオーサリングエクスペリエンスが中断されないようにするのに役立ちます。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

これで、`?ref=teaser` クエリパラメーターを追加する際に、ユニバーサルエディターで変更をプレビューできます。

![ユニバーサルエディターのティーザー](./assets/7a-block-css/universal-editor-preview.png)
