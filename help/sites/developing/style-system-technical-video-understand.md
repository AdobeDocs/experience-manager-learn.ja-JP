---
title: AEM スタイルシステムのコード作成方法について
description: このビデオでは、スタイルシステムを使用して Adobe Experience Manager のコアタイトルコンポーネントのスタイル設定に使用する CSS（または LESS）と JavaScript の構造を確認します。また、これらのスタイルが HTML と DOM にどのように適用されるかについても説明します。
feature: Style System
version: 6.4, 6.5, Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1061
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1065'
ht-degree: 100%

---

# スタイルシステム用のコード作成方法について{#understanding-how-to-code-for-the-aem-style-system}

このビデオでは、スタイルシステムを使用して Experience Manager のコアタイトルコンポーネントのスタイル設定に使用する CSS（または [!DNL LESS]）と JavaScript の構造を確認します。また、これらのスタイルが HTML と DOM にどのように適用されるかについても説明します。


## スタイルシステム用のコード作成方法について {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供された AEM パッケージ（**technical-review.sites.style-system-1.0.0.zip**）は、サンプルのタイトルスタイル、We.Retail レイアウトコンテナおよびタイトルコンポーネントのサンプルポリシー、サンプルページをインストールします。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

次に、以下の場所にあるサンプルスタイルの [!DNL LESS] 定義を示します。

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSS を好むユーザーの場合、このコードスニペットの下に、この [!DNL LESS] がコンパイルされる CSS があります。 

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

上記の [!DNL LESS] は、次の CSS に Experience Manager でネイティブにコンパイルされます。

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### JavaScript {#example-javascript}

次の JavaScript は、サンプルスタイルがタイトルコンポーネントに適用されている場合に、現在のページの最終更新日時を収集してタイトルテキストの下に挿入します。

jQuery の使用はオプションで、使用する命名規則も同様です。

次に、以下の場所にあるサンプルスタイルの [!DNL LESS] 定義を示します。

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## 開発のベストプラクティス {#development-best-practices}

### HTML のベストプラクティス {#html-best-practices}

* HTML（HTL から生成）は、できるだけ構造的にセマンティックである必要があります。要素の不要なグループ化やネストを回避します。
* HTML 要素は、BEM スタイルの CSS クラスを介してアドレス可能である必要があります。

**良い例** - コンポーネント内のすべての要素は、BEM 表記を使用してアドレス可能です。

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**悪い例** - リストとリスト要素は、要素名によってのみアドレス可能です。

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公開するためにバックエンド開発が必要な少量のデータを公開するよりも、より多くのデータを公開して非表示にする方が効果的です。

   * オーサリング可能なコンテンツの切り替えの実装は、この HTML を減らすのに役立ちます。作成者は、HTML に書き込むコンテンツ要素を選択できます。 すべてのスタイルで使用できるとは限らない HTML に画像を書き込む場合、これは特に重要です。
   * このルールの例外は、画像などの高価なリソースがデフォルトで公開される場合です。この場合、CSS により非表示にされたイベント画像が不必要に取得されるためです。

      * 最新の画像コンポーネントでは、多くの場合、JavaScript を使用して、ユースケース（viewport）に最も適した画像を選択して読み込みます。

### CSS のベストプラクティス {#css-best-practices}

>[!NOTE]
>
>スタイルシステムは、[BEM](https://en.bem.info/) で指定されているように、`BLOCK` と `BLOCK--MODIFIER` が同じ要素に適用されないという点で、[BEM](https://en.bem.info/) とは技術的に少し異なります。
>
>代わりに、製品の制約により、`BLOCK--MODIFIER` が `BLOCK` 要素の親に適用されます。
>
>[BEM](https://en.bem.info/) の他のすべてのテナントと連携する必要があります。

* [LESS](https://lesscss.org/)（AEM ネイティブでサポート）または [SCSS](https://sass-lang.com/)（カスタムビルドシステムが必要）などのプリプロセッサーを使用して、CSS の明確な定義と再利用性を実現します。

* セレクターの重み付け/特異性を均一に保ちます。これにより、特定が困難な CSS カスケードの競合を回避し、解決できます。
* 各スタイルを個別のファイルに整理します。
   * これらのファイルは、LESS／SCSS `@imports` を使用して組み合わせることができます。または、生の CSS が必要な場合は、HTML クライアントライブラリファイルを含めるか、カスタムフロントエンドアセットビルドシステムを使用して組み合わせることができます。
* 多くの複雑なスタイルを混在させないでください。
   * 1 つのコンポーネントに一度に適用できるスタイルが多いほど、順列の種類が多くなります。 これは、ブランドの整合性の維持、QA、確保が難しくなる可能性があります。
* CSS ルールを定義するには、常に CSS クラス（BEM 表記に従う）を使用してください。
   * CSS クラスを持たない要素（ベア要素）を選択する必要が絶対にある場合は、選択可能な CSS クラスを持つそのタイプの要素との競合よりも特異性が低いことを明示するために、それらを CSS 定義の上位に移動します。
* これはレスポンシブグリッドにアタッチされているので、`BLOCK--MODIFIER` のスタイルを直接指定しないでください。この要素の表示を変更すると、レスポンシブグリッドのレンダリングと機能に影響を与える可能性があるので、レスポンシブグリッドの動作を変更する目的がある場合のみ、このレベルでスタイルを設定してください。
* `BLOCK--MODIFIER` を使用してスタイル範囲を適用します。`BLOCK__ELEMENT--MODIFIERS` はコンポーネントで使用できますが、`BLOCK` はコンポーネントを表し、コンポーネントはスタイル設定されるものであるため、スタイルは「定義済み」で、`BLOCK--MODIFIER` によってスコープが指定されます。

CSS セレクターの構造の例を次に示します。

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第 1 レベルのセレクター</p> <p>BLOCK--MODIFIER</p> </td> 
   <td valign="bottom"><p>第 2 レベルセレクター</p> <p>ブロック</p> </td> 
   <td valign="bottom"><p>第 3 レベルのセレクター</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有効な CSS セレクター</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list--dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list--dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> color: blue;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image--hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image--hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color: red;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

コンポーネントがネストされている場合、これらのネストされたコンポーネント要素の CSS セレクターの深さは、第 3 レベルのセレクターを超えます。 ネストされたコンポーネントに対して同じパターンを繰り返しますが、スコープは親コンポーネントの `BLOCK` です。つまり、ネストされたコンポーネントの `BLOCK` を 3 番目のレベルで開始し、ネストされたコンポーネントの `ELEMENT` は 4 番目のセレクターレベルにあります。

### JavaScript のベストプラクティス {#javascript-best-practices}

このセクションで定義するベストプラクティスは、特に、機能的な目的ではなく、スタイル用にコンポーネントを操作することを目的とした「style-JavaScript」または JavaScript に関係します。

* Style-JavaScript は慎重に使用する必要があり、少数派のユースケースです。
* Style-JavaScript は主に、CSS によるスタイル設定をサポートするためにコンポーネントの DOM を操作するために使用する必要があります。
* コンポーネントがページに何度も表示される場合は Javascript の使用を再評価し、計算コストと再描画コストを理解します。
* コンポーネントがページに何度も表示される場合に、新しいデータ/コンテンツを（AJAX を介して）非同期で取り込む場合、JavaScript の使用を再評価します。
* パブリッシュエクスペリエンスとオーサリングエクスペリエンスの両方を処理します。
* 可能な場合は、style-JavaScript を再利用します。
   * 例えば、コンポーネントの複数のスタイルでその画像を背景画像に移動する必要がある場合、スタイル JavaScript を一度実装して、複数の `BLOCK--MODIFIERs` に添付できます。
* 可能な場合は、style-JavaScript と機能 JavaScript を区別します。
* HTL を介して直接 HTML でこれらの DOM 変更を示すのと比較して、JavaScript のコストを評価します。
   * style-JavaScript を使用するコンポーネントがサーバー側の変更を必要とする場合、この時点で JavaScript 操作を導入できるかどうか、およびコンポーネントのパフォーマンスとサポート可能性に対する影響は何かを評価します。

#### パフォーマンスに関する考慮事項 {#performance-considerations}

* Style-JavaScript は明るく、傾いている状態に保つ必要があります。
* ちらつきや不要な再描画を避けるために、最初は `BLOCK--MODIFIER BLOCK` でコンポーネントを非表示にし、JavaScript でのすべての DOM 操作が完了したら表示します。
* style-JavaScript 操作のパフォーマンスは、DOMReady 上の要素に関連付けて変更する基本的な jQuery プラグインと同じです。
* リクエストが gzip 圧縮され、CSS と JavaScript が縮小されていることを確認します。

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM クライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（ブロック要素修飾子）ドキュメント web サイト](https://getbem.com/)
* [LESS ドキュメント web サイト](https://lesscss.org/)
* [jQuery web サイト](https://jquery.com/)
