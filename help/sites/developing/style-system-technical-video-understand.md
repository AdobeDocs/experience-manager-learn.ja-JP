---
title: AEM Style System のコード作成方法について
description: このビデオでは、スタイルシステムを使用して Adobe Experience Manager のコアタイトルコンポーネントのスタイル設定に使用する CSS（または LESS）と JavaScript の構造、およびこれらのスタイルがHTMLと DOM にどのように適用されるかについて説明します。
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 2%

---

# スタイルシステムのコードを作成する方法について{#understanding-how-to-code-for-the-aem-style-system}

このビデオでは、CSS( または [!DNL LESS]) や JavaScript を使用して、スタイルシステムを使用して Experience Manager のコアタイトルコンポーネントのスタイル設定や、これらのスタイルがHTMLと DOM にどのように適用されるかを設定できます。


## スタイルシステムのコードを作成する方法について {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供されたAEMパッケージ (**technical-review.sites.style-system-1.0.0.zip**) は、サンプルのタイトルスタイル、We.Retail レイアウトコンテナおよびタイトルコンポーネントのサンプルポリシー、サンプルページをインストールします。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

次に、 [!DNL LESS] 次の場所にあるサンプルスタイルの定義：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSS を好むユーザーの場合、このコードスニペットの下に CSS が表示されます。 [!DNL LESS] をにコンパイルします。

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

上記 [!DNL LESS] は、次の CSS にExperience Managerでネイティブにコンパイルされます。

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

次の JavaScript は、タイトルコンポーネントにサンプルスタイルが適用されている場合に、タイトルテキストの下に現在のページの最終変更日時を収集し、挿入します。

jQuery の使用はオプションで、使用する命名規則も同様です。

次に、 [!DNL LESS] 次の場所にあるサンプルスタイルの定義：

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

### HTMLのベストプラクティス {#html-best-practices}

* HTML（HTL を介して生成）は、できるだけ構造的に意味的である必要があります。要素の不要なグループ化やネストを回避する。
* HTML要素は、BEM スタイルの CSS クラスを介してアドレス可能にする必要があります。

**良い**  — コンポーネント内のすべての要素は、BEM 表記を使用してアドレス可能です。

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**悪い**  — リスト要素とリスト要素は、要素名でのみアドレス可能です。

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公開するバックエンド開発を必要とするデータをあまりに少なく公開するよりも、より多くのデータを公開して非表示にする方が効果的です。

   * 作成者可能なコンテンツの切り替えの実装は、このHTMLを減らすのに役立ちます。作成者は、HTMLに書き込むコンテンツ要素を選択できます。 は、特に、すべてのスタイルに使用されない可能性のあるHTMLに画像を書き込む際に重要です。
   * このルールの例外は、CSS で非表示のイベント画像が不必要に取得されるので、高価なリソース（画像など）がデフォルトで公開される場合です。

      * 最新の画像コンポーネントでは、多くの場合、JavaScript を使用して、ユースケース（ビューポート）に最も適した画像を選択して読み込みます。

### CSS のベストプラクティス {#css-best-practices}

>[!NOTE]
>
>スタイルシステムは、少し技術的な相違を生じます [BEM](https://en.bem.info/)、 `BLOCK` および `BLOCK--MODIFIER` は、 [BEM](https://en.bem.info/).
>
>代わりに、製品の制約により、 `BLOCK--MODIFIER` が `BLOCK` 要素。
>
>その他すべてのテナント [BEM](https://en.bem.info/) は、と整列する必要があります。

* 次のようなプリプロセッサーを使用： [LESS](https://lesscss.org/) (AEMネイティブでサポート ) または [SCSS](https://sass-lang.com/) （カスタムビルドシステムが必要）を使用して、CSS の明確な定義と再利用性を実現する。

* セレクターの重み/特異性を一定に保つ。これにより、CSS のカスケード競合を回避し、識別しにくい問題を解決できます。
* 各スタイルを個別のファイルに整理します。
   * LESS/SCSS を使用してこれらのファイルを組み合わせることができます `@imports` 生の CSS が必要な場合は、HTMLクライアントライブラリファイルを含めるか、カスタムフロントエンドアセットビルドシステムを使用します。
* 多くの複雑なスタイルを混在させないでください。
   * 1 つのコンポーネントに一度に適用できるスタイルが多いほど、順列の種類が多くなります。 これは、ブランドの整合性の維持、QA、確保が難しくなる可能性があります。
* 常に CSS クラス（BEM 表記に従う）を使用して、CSS ルールを定義します。
   * CSS クラスを持たない要素（ベア要素）を選択する必要が絶対にある場合は、CSS 定義の要素を高くして、選択可能な CSS クラスを持つそのタイプの要素との競合よりも特異性が低いことを明確に示します。
* のスタイル設定を避ける `BLOCK--MODIFIER` レスポンシブグリッドにアタッチされるときに、直接。 この要素の表示を変更すると、レスポンシブグリッドのレンダリングと機能に影響を与える可能性があるので、レスポンシブグリッドの動作を変更する目的がある場合のみ、このレベルのスタイルを設定できます。
* 次を使用してスタイル範囲を適用 `BLOCK--MODIFIER`. この `BLOCK__ELEMENT--MODIFIERS` はコンポーネントで使用できますが、 `BLOCK` はコンポーネントを表し、コンポーネントはスタイル設定されたもので、スタイルは「定義済み」で、スコープはを使用して設定されます `BLOCK--MODIFIER`.

CSS セレクターの構造の例を次に示します。

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第 1 レベルのセレクター</p> <p>ブロック — 修飾子</p> </td> 
   <td valign="bottom"><p>第 2 レベルセレクター</p> <p>ブロック</p> </td> 
   <td valign="bottom"><p>第 3 レベルのセレクター</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有効な CSS セレクター</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> 色：青い。</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> 色：赤</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

コンポーネントがネストされている場合、これらのネストされたコンポーネント要素の CSS セレクターの深さは、第 3 レベルのセレクターを超えます。 親コンポーネントの `BLOCK`. つまり、ネストされたコンポーネントの `BLOCK` 3 番目のレベル、およびネストされたコンポーネントの `ELEMENT` が 4 番目のセレクターレベルにある。

### JavaScript のベストプラクティス {#javascript-best-practices}

この節で定義するベストプラクティスは、特に、機能的な目的ではなく、スタイル用にコンポーネントを操作することを目的とした「style-JavaScript」または JavaScript に関係します。

* Style-JavaScript は慎重に使用する必要があり、少数派の使用例です。
* Style-JavaScript は主に、CSS によるスタイル設定をサポートするためにコンポーネントの DOM を操作するために使用する必要があります。
* コンポーネントがページ上に複数回表示される場合の JavaScript の使用を再評価し、計算/描画のコストを把握します。
* コンポーネントがページに複数回表示される場合に、新しいデータ/コンテンツを (AJAXを介して ) 非同期で取り込む場合、JavaScript の使用を再評価します。
* 公開エクスペリエンスとオーサリングエクスペリエンスの両方を処理します。
* 可能な場合は、style-JavaScript を再利用します。
   * 例えば、1 つのコンポーネントの複数のスタイルで画像を背景画像に移動する必要がある場合、style-JavaScript は 1 回実装して複数のスタイルに添付できます `BLOCK--MODIFIERs`.
* 可能な場合は、style-JavaScript と機能 JavaScript を区別します。
* HTL を介して直接HTMLでこれらの DOM 変更を示すのと比較して、JavaScript のコストを評価します。
   * style-JavaScript を使用するコンポーネントでサーバー側の変更が必要な場合は、JavaScript 操作がその時点で取り込めるかどうか、およびコンポーネントのパフォーマンスとサポート性に与える影響と影響を評価します。

#### パフォーマンスに関する考慮事項 {#performance-considerations}

* Style-JavaScript は明るく、傾いている状態に保つ必要があります。
* ちらつきや不要な再描画を避けるために、最初はを介してコンポーネントを非表示にします。 `BLOCK--MODIFIER BLOCK`JavaScript 内のすべての DOM 操作が完了したら表示します。
* style-JavaScript 操作のパフォーマンスは、DOMReady 上の要素に関連付けて変更する基本的な jQuery プラグインと同じです。
* リクエストが gzip 圧縮され、CSS と JavaScript が縮小されていることを確認します。

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEMクライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（ブロック要素修飾子）ドキュメント Web サイト](https://getbem.com/)
* [LESS ドキュメント Web サイト](https://lesscss.org/)
* [jQuery Web サイト](https://jquery.com/)
