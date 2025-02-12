---
title: ブロックオプション
description: 複数の表示オプションを持つブロックを作成する方法を説明します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-17296
duration: 700
exl-id: f41dff22-bd47-4ea0-98cc-f5ca30b22c4b
source-git-commit: ae3ade0f31846776aa9bdd3a615d6514b626f48d
workflow-type: tm+mt
source-wordcount: '1958'
ht-degree: 12%

---

# オプション付きブロックの開発

このチュートリアルは、Edge Delivery Servicesとユニバーサルエディターのチュートリアルに基づいて構築され、ブロックにブロックオプションを追加するプロセスをガイドします。 ブロックオプションを定義すると、ブロックの外観と機能をカスタマイズして、様々なコンテンツニーズに合わせて様々なバリエーションを作成できます。 これにより、サイトのデザインシステム内での柔軟性と再利用性が向上します。

![ 並列ブロックオプション ](./assets/block-options/main.png){align="center"}

このチュートリアルでは、ティーザーブロックにブロックオプションを追加します。作成者は、**デフォルト** と **並べて表示** の 2 つの表示オプションから選択できます。 **デフォルト** オプションは、テキストの前後に画像を表示するのに対し、**左右に並べる** オプションは、画像とテキストを左右に並べて表示します。

## 一般的なユースケース

**2}Edge Delivery Services** および **ユニバーサルエディター** 開発で { ブロックオプション **を使用する一般的なユースケースには、以下が含まれますが、これらに限定されません。**

1. **レイアウトバリエーション：** レイアウトを簡単に切り替えることができます。 例えば、水平と垂直、グリッドとリストの比較などです。
2. **スタイルのバリエーション：** テーマやビジュアル処理を簡単に切り替えることができます。 例えば、明るいモードと暗いモードの比較や、大きいテキストと小さいテキストの比較などです。
3. **コンテンツの表示コントロール：** 要素の表示を切り替えたり、コンテンツスタイルを（コンパクトと詳細で）切り替えたりします。

これらのオプションは、動的で適応可能なブロックを構築する際の柔軟性と効率を提供します。

このチュートリアルでは、レイアウトバリエーションのユースケースについて説明します。ティーザーブロックは、**デフォルト** と **並べて表示** の 2 つの異なるレイアウトで表示できます。

## ブロックモデル

ティーザーブロックにブロックオプションを追加するには、`/block/teaser/_teaser.json` の JSON フラグメントを開き、新しいフィールドをモデル定義に追加します。 このフィールドは、`name` プロパティを `classes` に設定します。これは、AEMでブロックオプションの保存に使用される保護されたフィールドであり、ブロックのEdge Delivery Services HTMLに適用されます。

### フィールド設定

以下のタブでは、単一の CSS クラスを使用した単一選択、複数の CSS クラスを使用した単一選択、複数の CSS クラスを使用した複数選択など、ブロックモデルでブロックオプションを設定する様々な方法を説明します。 このチュートリアルは [ 単一の CSS クラスを使用した選択 ](#field-configuration-for-this-tutorial) で使用される **よりシンプルなアプローチを実装** しています。

>[!BEGINTABS]

>[!TAB  単一の CSS クラスで選択 ]

このチュートリアルでは、`select` （ドロップダウン）入力タイプを使用して、作成者が 1 つのブロックオプションを選択できるようにする方法を説明します。このオプションは、対応する単一の CSS クラスとして適用されます。

![ 単一の CSS クラスで選択 ](./assets/block-options/tab-1.png){align="center"}

#### ブロックモデル

**デフォルト** オプションは空の文字列（`""`）で表され、「**並べて表示** オプションは `"side-by-side"` を使用します。 オプションの **name** と **value** は同じである必要はありませんが、**value** はブロックのHTMLに適用される CSS クラスを指定します。 例えば、「**左右に並べて**」オプションの値は、`side-by-side` ではなく `layout-10` にすることができます。 ただし、CSS クラスには意味論的に意味のある名前を使用し、オプション値の明確さと一貫性を確保することをお勧めします。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json{highlight="4,8,9-18"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            }
        ]
    }
]
...
```

#### ブロックの HTML

作成者がオプションを選択すると、対応する値が CSS クラスとしてブロックのHTMLに追加されます。

- **デフォルト** が選択されている場合：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- 「**並べて表示**」が選択されている場合：

  ```html
  <div class="block teaser side-by-side">
      <!-- Block content here -->
  </div>
  ```

これにより、選択した開封に応じて、異なるスタイル設定と条件付きJavaScriptを適用できます。


>[!TAB  複数の CSS クラスで選択 ]

**この方法は、このチュートリアルでは使用しませんが、代替方法と高度なブロックオプションを示しています。**

`select` 入力タイプを使用すると、作成者は、1 つのブロックオプションを選択できます。このオプションは、オプションで複数の CSS クラスにマッピングできます。 これを行うには、CSS クラスをスペース区切り値としてリストします。

![ 複数の CSS クラスで選択 ](./assets/block-options/tab-2.png){align="center"}

#### ブロックモデル

例えば、「**並べて表示** オプションでは、画像を左に表示（`side-by-side left`）したり、右に表示（`side-by-side right`）したりするバリエーションをサポートできます。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json{highlight="4,8,9-21"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side with Image on left",
                "value": "side-by-side left"
            },
            {
                "name": "Side-by-side with Image on right",
                "value": "side-by-side right"
            }
        ]
    }
]
...
```

#### ブロックの HTML

作成者がオプションを選択すると、対応する値が、ブロックのHTML内で CSS クラスのスペース区切りセットとして適用されます。

- **デフォルト** が選択されている場合：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- **画像を左に並べて** が選択されている場合：

  ```html
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- **画像を右に並べて** が選択されている場合：

  ```html
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

これにより、選択したオプションに応じて異なるスタイル設定と条件付きJavaScriptを適用できます。


>[!TAB  複数の CSS クラスによる複数選択 ]

**この方法は、このチュートリアルでは使用しませんが、代替方法と高度なブロックオプションを示しています。**

`"component": "multiselect"` の入力タイプを使用すると、作成者は複数のオプションを同時に選択できます。 これにより、複数のデザイン選択を組み合わせて、ブロックの外観を複雑に並べ替えることができます。

![ 複数の CSS クラスによる複数選択 ](./assets/block-options/tab-3.png){align="center"}

### ブロックモデル

例えば、**左右に並べる**、**左に画像** および **右に画像** は、画像を左（`side-by-side left`）または右（`side-by-side right`）に配置するバリエーションをサポートできます。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json{highlight="4,6,8,10-21"}
...
"fields": [
    {
        "component": "multiselect",
        "name": "classes",
        "value": [],
        "label": "Teaser options",
        "valueType": "array",
        "options": [
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            },
            {
                "name": "Image on left",
                "value": "left"
            },
            {
                "name": "Image on right",
                "value": "right"
            }
        ]
    }
]
...
```

#### ブロックの HTML

作成者が複数のオプションを選択すると、対応する値がブロックのHTMLでスペース区切りの CSS クラスとして適用されます。

- **左右に並べて表示** および **左側に画像** が選択されている場合：

  ```html{highlight="1"}
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- **左右に並べて** および **右側の画像** が選択されている場合：

  ```html{highlight="1"}
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

複数選択には柔軟性がありますが、デザインの並べ替えの管理は複雑になります。 制限がないと、選択が競合して、エクスペリエンスが壊れたり、ブランドに反したりする可能性があります。

例：

- **画像を左に配置** または **画像を右に配置** を選択せずに **左右に並べて配置** すると、画像を常に背景として設定する **デフォルト** に暗黙的に適用されるので、左右の位置揃えは関係ありません。
- **左の画像** と **右の画像** の選択は矛盾しています。
- 画像の位置が特定できないため、**左の画像** または **右の画像** を使用せずに **左右に並べて** 選択するとあいまいであると見なされる場合があります。

問題を防ぎ、複数選択を使用する際の作成者の混乱を防ぐには、オプションを適切に計画し、すべての並べ替えをテストします。 複数選択は、レイアウトを変更する選択肢ではなく、「大きい」や「ハイライト」など、競合しない単純な機能強化に最適です。


>[!TAB  デフォルトオプション ]

**この方法は、このチュートリアルでは使用しませんが、代替方法と高度なブロックオプションを示しています。**

ユニバーサルエディターで新しいブロックインスタンスをページに追加する際に、ブロックオプションをデフォルトとして設定できます。 これは、[ ブロックの定義 ](../5-new-block.md#block-definition) の `classes` プロパティの既定値を設定することによって行います。

#### ブロックの定義

次の例では、デフォルトのオプションは、`classes` フィールドの `value` プロパティを `side-by-side` に割り当てることによって **Side-by-Side** に設定されています。 ブロックモデルの対応するブロックオプション入力はオプションです。

また、同じブロックに対して、それぞれ異なる名前とクラスを持つ複数のエントリを定義することもできます。 これにより、ユニバーサルエディターは個別のブロックエントリを表示できます。各ブロックエントリは、特定のブロックオプションで事前設定されます。 これらはエディターでは別々のブロックとして表示されますが、コードベースには、選択したオプションに基づいて動的にレンダリングされる単一のブロックが含まれています。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json{highlight="12"}
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "classes": "side-by-side",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

>[!ENDTABS]


### このチュートリアルのフィールド設定


このチュートリアルでは、上記の最初のタブで説明した、単一の CSS クラスを使用して選択するアプローチを使用します。これにより、**デフォルト** と **並べて表示** という 2 つの個別のブロックオプションが可能になります。

ブロックの JSON フラグメント内のモデル定義で、ブロックオプション用の選択フィールドを 1 つ追加します。 このフィールドを使用すると、作成者はデフォルトのレイアウトと左右に並べられたレイアウトの中から選択することができます。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json{highlight="7-24"}
{
    "definitions": [...],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "select",
                    "name": "classes",
                    "value": "",
                    "label": "Teaser options",
                    "description": "",
                    "valueType": "string",
                    "options": [
                        {
                            "name": "Default",
                            "value": ""
                        },
                        {
                            "name": "Side-by-side",
                            "value": "side-by-side"
                        }
                    ]
                },
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

## ユニバーサルエディターの更新ブロック

更新されたブロックオプション入力をユニバーサルエディターで使用できるようにするには、JSON コードの変更を GitHub にデプロイし、新しいページを作成して、「**並べて**」オプションを使用してティーザーブロックを追加および作成し、ページをプレビュー用に公開します。 公開したら、ページをローカル開発環境に読み込んでコーディングします。

### 変更を GitHub にプッシュ

更新したブロックオプション入力をユニバーサルエディターで使用して、ブロックオプションを設定し、結果のHTMLに対して開発できるようにするには、プロジェクトをリンクし、変更を GitHub ブランチ（この場合は `block-options` ブランチ）にプッシュする必要があります。

```bash
# ~/Code/aem-wknd-eds-ue

# Lint the changes to catch any syntax errors
$ npm run lint 

$ git add .
$ git commit -m "Add Teaser block option to JSON file so it is available in Universal Editor"
$ git push origin teaser
```

### テストページの作成

AEM オーサーサービスで、新しいページを作成して、開発用のティーザーブロックを追加します。 [2}Edge Delivery Servicesとユニバーサルエディターのデベロッパーチュートリアル ](../6-author-block.md) ブロックの作成 ](../0-overview.md) の章の規則に従って、`branches` ページの下にテストページを作成し、作業中の Git ブランチの後に名前を付けます（この場合は `block-options`）。[

### ブロックのオーサリング

ユニバーサルエディターで新しい **ブロックオプション** ページを編集し、「**ティーザー** ブロックを追加します。 `block-options` GitHub ブランチのコードを使用してページを読み込むには、URL にクエリパラメーター `?ref=block-options` を追加します。

ブロックダイアログに、**デフォルト** と **左右に並べて** の選択肢を含む **ティーザーオプション** ドロップダウンが含まれるようになりました。 「**並べて表示**」を選択して、残りのコンテンツのオーサリングを完了します。

![ オプションブロックダイアログ付きティーザー ](./assets/block-options/block-dialog.png){align="center"}

必要に応じて、2 つの **ティーザー** ブロックを追加します。1 つは **デフォルト** に、もう 1 つは **並べて** に設定します。 これにより、開発時に両方のオプションを並べてプレビューできるので、**並べて** 実装しても **デフォルト** オプションに影響を与えません。

### プレビュー用に公開

ティーザーブロックがページに追加されたら、[ パブリケーションの管理 **およびAEM オーサーのサイト管理を使用して ](../6-author-block.md) プレビュー用にページを公開** します。

## ブロックの HTML

ブロックの開発を開始するには、まず、Edge Delivery Servicesのプレビューで公開される DOM 構造を確認します。 DOM はJavaScriptで強化され、CSS でスタイル設定され、ブロックを作成およびカスタマイズするための基盤を提供します。

>[!BEGINTABS]

>[!TAB 装飾する DOM]

次に、ティーザーブロックの DOM を示します。「**並べて表示**」ブロックオプションを選択した状態で、JavaScriptと CSS を使用してデコレートします。

```html{highlight="7"}
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block side-by-side" data-block-name="teaser" data-block-status="loaded">
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
                    <p>Terms and conditions: By signing up, you agree to the rules for participation and booking.</p>
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

デコレートする DOM を探すには、ローカル開発環境でブロックを含んだページを開き、web ブラウザーのデベロッパーツールを使用してブロックを選択し、DOM を調べます。 これにより、装飾する関連する要素を特定できます。

![ブロックの DOM の検査](./assets/block-options/dom.png){align="center"}

>[!ENDTABS]

## ブロックの CSS

`blocks/teaser/teaser.css` を編集して、「**並べて表示**」オプションに特定の CSS スタイルを追加します。 このファイルには、ブロックのデフォルトの CSS が含まれています。

「**並べて表示**」オプションのスタイルを変更するには、`side-by-side` クラスで設定されたティーザーブロックをターゲットにする、スコープ指定された新しい CSS ルールを `teaser.css` ファイルに追加します。

```css
.block.teaser.side-by-side { ... }
```

または、CSS ネストを使用して、より簡潔なバージョンを作成することもできます。

```css
.block.teaser {
    ... Default teaser block styles ...

    &.side-by-side {
        ... Side-by-side teaser block styles ...
    }
}
```

`&.side-by-side` ルール内で、必要な CSS プロパティを追加して、`side-by-side` クラスが適用されるときにブロックのスタイルを設定します。

一般的なアプローチは、`all: initial` を共有セレクターに適用してデフォルトのスタイルをリセットし、`side-by-side` のバリアントに必要なスタイルを追加することです。 ほとんどのスタイルがオプション間で共有されている場合は、特定のプロパティを上書きする方が簡単な場合があります。 ただし、複数のセレクターを変更する必要がある場合は、すべてのスタイルをリセットし、必要なスタイルのみを再適用すると、コードがより明確でメンテナンスしやすくなる可能性があります。
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
    overflow: hidden; 


    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        }

        p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
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

    /**
    *  Add styling for the side-by-side variant 
    **/

    /* This evaluates to .block.teaser.side-by-side */
    &.side-by-side {    
        /* Since this default teaser option doesn't have a style (such as `.default`), we use `all: initial` to reset styles rather than overriding individual styles. */
        all: initial;
        display: flex;
        margin: auto;
        max-width: 900px;

        .image-wrapper {
            all: initial;
            flex: 2;
            overflow: hidden;                 
            
            * {
                height: 100%;
            }        

            .image {
                object-fit: cover;
                object-position: center;
                width: 100%;
                height: 100%;
                transform: scale(1); 
                transition: transform 0.6s ease-in-out;                

                &.zoom {
                    /* This option has a different zoom level than the default */
                    transform: scale(1.5);
                }
            }
        }

        .content {
            all: initial;
            flex: 1;
            background-color: var(--light-color);
            padding: 3.5em 2em 2em;
            font-size: var(--body-font-size-s);
            font-family: var(--body-font-family);
            text-align: justify;
            text-justify: newspaper;
            hyphens: auto;

            p.terms-and-conditions {
                border: solid var(--text-color);
                border-width: 0;
                padding-left: 0;
                text-align: left;
            }
        }

        /* Media query for mobile devices */
        @media (width <= 900px) {
            flex-direction: column; /* Stack elements vertically on mobile */
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


## ブロックの JavaScript

ブロック要素に適用されるクラスを確認すると、ブロックのアクティブなオプションを簡単に識別できます。 この例では、アクティブなオプションに応じて `.image-wrapper` スタイルを適用する場所を調整する必要があります。

`getOptions` 関数は、`block` と `teaser` を除く、ブロックに適用されたクラスの配列を返します（すべてのブロックは `block` クラスを持ち、すべてのティーザーブロックは `teaser` クラスを持つため）。 配列内の残りのクラスは、アクティブなオプションを示します。 配列が空の場合は、デフォルトのオプションが適用されます。

```javascript
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'; anything remaining is a block option.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}
```

このオプションリストを使用すると、ブロックのJavaScriptでカスタムロジックを条件付きで実行できます。

```javascript
if (getOptions(block).includes('side-by-side')) {
  /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
  block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
} else if (!getOptions(block)) {
  /* For the default option, add the image-wrapper to the picture element to support CSS */
  block.querySelector('picture').classList.add('image-wrapper');
}
```

デフォルトと左右に並べて表示される両方のオプションを含む、ティーザーブロックの完全に更新されたJavaScript ファイルを次に示します。

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Block options are applied as classes to the block's DOM element
 * alongside the `block` and `<block-name>` classes.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}

/**
 * Adds a zoom effect to the image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's DOM tree
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
 * Entry point to the block's JavaScript.
 * Must be exported as default and accept a block's DOM element.
 * This function is called by the project's style.js, passing the block's element.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
export default function decorate(block) {
  /* Common treatments for all options */
  block.querySelector(':scope > div:last-child').classList.add('content');
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');
  block.querySelector('img').classList.add('image');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();
    if (innerHTML?.startsWith('Terms and conditions:')) {
      p.classList.add('terms-and-conditions');
    }
  });

  /* Conditional treatments for specific options */
  if (getOptions(block).includes('side-by-side')) {
    /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
    block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
  } else if (!getOptions(block)) {
    /* For the default option, add the image-wrapper to the picture element to support CSS */
    block.querySelector('picture').classList.add('image-wrapper');
  }

  addEventListeners(block);
}
```

## 開発プレビュー

CSS と JavaScript を追加すると、AEM CLI のローカル開発環境によって変更がホットリロードされ、コードがブロックに与える影響をすばやく簡単に視覚化できます。CTA にポインタを合わせて、ティーザーの画像がズームインおよびズームアウトされることを確認します。

![CSS および JS を使用したティーザーのローカル開発プレビュー](./assets/block-options//local-development-preview.png)

## コードのリント

コードの変更をクリーンで一貫性のある状態に保つには、[頻繁にリント](../3-local-development-environment.md#linting)します。定期的なリンティングを行うと、問題を早期に発見し、全体的な開発時間を短縮できます。すべてのリンティングの問題が解決されるまで、開発作業を `main` 分岐に結合できません。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## ユニバーサルエディターでのプレビュー

AEM のユニバーサルエディターで変更を表示するには、ユニバーサルエディターで使用される Git リポジトリ分岐に変更を追加、コミット、プッシュします。これにより、ブロックの実装によってオーサリングエクスペリエンスが中断されなくなります。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Teaser block option Side-by-side"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin block-options
```

`?ref=block-options` クエリパラメーターを使用すると、変更がユニバーサルエディターに表示されるようになりました。

![ユニバーサルエディターのティーザー](./assets/block-options/universal-editor-preview.png){align="center"}


## おめでとうございます。

これで、Edge Delivery Servicesとユニバーサルエディターのブロックオプションについて説明し、柔軟にコンテンツ編集をカスタマイズおよび効率化できるツールを提供しました。 効率を向上させ一貫性を維持するために、プロジェクトでこれらのオプションの適用を開始します。

その他のベストプラクティスと高度な手法については、[ ユニバーサルエディターのドキュメント ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options) を参照してください。
