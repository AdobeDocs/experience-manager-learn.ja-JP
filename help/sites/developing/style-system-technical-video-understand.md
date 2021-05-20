---
title: AEMスタイルシステムのコード作成方法について
description: このビデオでは、スタイルシステムを使用してAdobe Experience Managerのコアタイトルコンポーネントのスタイル設定に使用するCSS（またはLESS）とJavaScriptの構造と、これらのスタイルがHTMLおよびDOMにどのように適用されるかについて説明します。
feature: スタイルシステム
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: 開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1148'
ht-degree: 4%

---


# スタイルシステムのコーディング方法について{#understanding-how-to-code-for-the-aem-style-system}

このビデオでは、Experience Managerのコアタイトルコンポーネントのスタイル設定に使用されるCSS（または[!DNL LESS]）とJavaScriptの構造と、これらのスタイルがHTMLおよびDOMにどのように適用されるかについて説明します。

>[!NOTE]
>
>AEMスタイルシステムは、[AEM 6.3 SP1](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp1-release-notes.html) + [機能パック20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)で導入されました。
>
>このビデオでは、 We.Retailタイトルコンポーネントが更新され、[コアコンポーネントv2.0.0以降](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)を継承していることを前提としています。

## スタイルシステムのコード化方法について{#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

提供されているAEMパッケージ(**technical-review.sites.style-system-1.0.0.zip**)では、サンプルのタイトルスタイル、We.Retailレイアウトコンテナおよびタイトルコンポーネントのサンプルポリシー、およびサンプルページがインストールされます。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

次に、にあるサンプルスタイルの[!DNL LESS]定義を示します。

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSSを好むユーザーの場合、このコードスニペットの下に、この[!DNL LESS]によってコンパイルされるCSSが表示されます。

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

上記の[!DNL LESS]は、次のCSSにExperience Managerでネイティブにコンパイルされます。

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

次のJavaScriptは、サンプルスタイルがタイトルコンポーネントに適用されている場合に、現在のページの最終変更日時をタイトルテキストの下に収集し、挿入します。

jQueryの使用はオプションで、使用する命名規則も同様です。

次に、にあるサンプルスタイルの[!DNL LESS]定義を示します。

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

## 開発のベストプラクティス{#development-best-practices}

### HTMLのベストプラクティス{#html-best-practices}

* HTML（HTLを使用して生成）は、できるだけ構造的に意味のあるものにする必要があります。要素の不要なグループ化やネストを回避する。
* HTML要素は、BEMスタイルのCSSクラスを使用してアドレス可能にする必要があります。

**良い**  — コンポーネント内のすべての要素は、BEM表記でアドレス可能です。

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**不良**  — リスト要素とリスト要素は要素名でのみアドレス可能です。

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公開するバックエンド開発を必要とするデータを少なくして公開するよりも、公開するデータを増やして非表示にする方が効果的です。

   * 作成者がHTMLをリーンな状態に保つのに役立つオーサー可能なコンテンツトグルの実装。オーサー可能なコンテンツトグルを使用すると、作成者は、HTMLに書き込むコンテンツ要素を選択できます。 は、特に、すべてのスタイルで使用されるとは限らない、HTMLに画像を書き込む場合に重要になります。
   * このルールの例外は、CSSで非表示のイベント画像は必要なく取得されるので、高価なリソース（画像など）がデフォルトで公開される場合です。

      * 最新の画像コンポーネントでは、多くの場合、JavaScriptを使用して、ユースケース（ビューポート）に最適な画像を選択して読み込みます。

### CSSのベストプラクティス{#css-best-practices}

>[!NOTE]
>
>[BEM](https://en.bem.info/)で指定されたように`BLOCK`と`BLOCK--MODIFIER`が同じ要素に適用されない点で、スタイルシステムは[BEM](https://en.bem.info/)とは技術的に多少異なります。
>
>代わりに、製品の制約により、`BLOCK--MODIFIER`が`BLOCK`要素の親に適用されます。
>
>[BEM](https://en.bem.info/)のその他すべてのテナントは、と整合する必要があります。

* CSSの定義を明確にし、再利用性を高めるために、 [LESS](https://lesscss.org/) (AEMでネイティブにサポート)や[SCSS](https://sass-lang.com/) （カスタムビルドシステムが必要）などのプリプロセッサーを使用します。

* セレクターの重み/特異性を均一にする。これにより、識別が困難なCSSカスケードの競合を回避し、解決できます。
* 各スタイルを個別のファイルに編成します。
   * LESS/SCSS `@imports`を使用して、または生のCSSが必要な場合は、HTMLクライアントライブラリファイルを含めるか、カスタムフロントエンドアセットビルドシステムを使用して、これらのファイルを組み合わせることができます。
* 多くの複雑なスタイルを混在させないでください。
   * 1つのコンポーネントに一度に適用できるスタイルが多いほど、順列の種類が多くなります。 これは、ブランドの整合性の維持/QA/確保が難しくなる可能性があります。
* 常にCSSクラス（BEM表記に従う）を使用して、CSSルールを定義します。
   * CSSクラスを持たない要素（つまり、ベア要素）を選択する必要が絶対にある場合は、CSS定義でそれらの要素を上に移動して、選択可能なCSSクラスを持つそのタイプの要素との競合よりも特異性が低いことを明確にします。
* `BLOCK--MODIFIER`はレスポンシブグリッドに添付されるので、直接スタイルを設定しないでください。 この要素の表示を変更すると、レスポンシブグリッドのレンダリングと機能に影響を与える可能性があるので、レスポンシブグリッドの動作を変更する目的がある場合にのみ、このレベルのスタイルを設定できます。
* `BLOCK--MODIFIER`を使用してスタイル範囲を適用します。 `BLOCK__ELEMENT--MODIFIERS`はコンポーネントで使用できますが、`BLOCK`はコンポーネントを表し、コンポーネントはスタイル設定されるので、スタイルは`BLOCK--MODIFIER`を介して「定義」され、スコープされます。

CSSセレクターの構造の例を次に示します。

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1レベルセレクタ</p> <p>BLOCK:MODIFIER</p> </td> 
   <td valign="bottom"><p>第2レベルセレクター</p> <p>ブロック</p> </td> 
   <td valign="bottom"><p>第3レベルセレクター</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有効なCSSセレクター</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> 色：青。</strong></p> <p><strong> }</strong></p> </td> 
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

コンポーネントがネストされている場合、これらのネストされたコンポーネント要素のCSSセレクターの深さは、第3レベルのセレクターを超えます。 ネストされたコンポーネントに対して同じパターンを繰り返しますが、親コンポーネントの`BLOCK`で範囲が指定されます。 つまり、ネストされたコンポーネントの`BLOCK`を3番目のレベルで開始し、ネストされたコンポーネントの`ELEMENT`を4番目のセレクターレベルで開始します。

### JavaScriptのベストプラクティス{#javascript-best-practices}

この節で定義するベストプラクティスは、「スタイルJavaScript」、つまり、機能的な目的ではなく、スタイル用にコンポーネントを操作することを目的としたJavaScriptに関係します。

* Style-JavaScriptは慎重に使用する必要があり、少数派の使用例です。
* Style-JavaScriptは、主に、CSSによるスタイル設定をサポートするためにコンポーネントのDOMを操作するために使用する必要があります。
* コンポーネントがページ上に何度も表示される場合にJavaScriptの使用を再評価し、計算/再描画のコストを把握します。
* コンポーネントがページに複数回表示される場合に、新しいデータ/コンテンツを(AJAXを介して)非同期で取り込む場合にJavaScriptの使用を再評価します。
* 公開エクスペリエンスとオーサリングエクスペリエンスの両方を処理します。
* 可能な場合は、style-JavaScriptを再使用します。
   * 例えば、1つのコンポーネントの複数のスタイルで画像を背景画像に移動する必要がある場合、style-JavaScriptを1回実装して複数の`BLOCK--MODIFIERs`に添付できます。
* 可能な場合は、style-JavaScriptと機能JavaScriptを区切ります。
* HTLを介してHTMLでこれらのDOM変更をJavaScriptのコストとマニフェストの両方を直接評価します。
   * スタイルJavaScriptを使用するコンポーネントでサーバー側の変更が必要な場合は、JavaScript操作がその時点で取り込めるかどうか、およびコンポーネントのパフォーマンスとサポートに与える影響と影響を評価します。

#### パフォーマンスに関する考慮事項{#performance-considerations}

* Style-JavaScriptは明るく、傾いた状態に保つ必要があります。
* ちらつきや不要な再描画を避けるために、最初に`BLOCK--MODIFIER BLOCK`を介してコンポーネントを非表示にし、JavaScriptでのすべてのDOM操作が完了したら表示します。
* スタイルとJavaScriptの操作のパフォーマンスは、DOMReady上の要素にアタッチして変更する基本的なjQueryプラグインと似ています。
* リクエストがgzip圧縮され、CSSとJavaScriptが縮小されていることを確認します。

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEMクライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（ブロック要素修飾子）ドキュメントWebサイト](https://getbem.com/)
* [LESSドキュメントWebサイト](https://lesscss.org/)
* [jQuery Webサイト](https://jquery.com/)
