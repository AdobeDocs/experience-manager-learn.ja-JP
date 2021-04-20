---
title: AEMスタイルシステムのコードの作成方法について
description: このビデオでは、スタイルシステムを使用してAdobe Experience Managerのコアタイトルコンポーネントのスタイル設定に使用するCSS（またはLESS）とJavaScriptの構造、およびこれらのスタイルがHTMLとDOMにどのように適用されるかを見てみます。
feature: スタイルシステム
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: 開発
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1148'
ht-degree: 4%

---


# スタイルシステムのコードの作成方法{#understanding-how-to-code-for-the-aem-style-system}

このビデオでは、Experience Managerのコアタイトルコンポーネントのスタイル設定に使用するCSS（または[!DNL LESS]）とJavaScriptの構造、およびスタイルシステムを使用したこれらのスタイルのHTMLとDOMへの適用方法を説明します。

>[!NOTE]
>
>AEMスタイルシステムは、[AEM 6.3 SP1](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp1-release-notes.html) + [機能パック20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)で導入されました。
>
>このビデオでは、We.Retail Titleコンポーネントが[コアコンポーネントv2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)を継承するように更新されたことを前提としています。

## スタイルシステムのコードの作成方法について{#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

提供されるAEMパッケージ(**technical-review.sites.style-system-1.0.0.zip**)では、サンプルのタイトルスタイル、Web.Retail Layoutコンテナとタイトルコンポーネント用のサンプルポリシー、およびサンプルページがインストールされます。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

次に、にある例のスタイルの[!DNL LESS]定義を示します。

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSSを好むユーザーの場合、このコードスニペットの下にこの[!DNL LESS]がコンパイルするCSSが表示されます。

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

上記の[!DNL LESS]は、次のCSSにExperience Managerしてネイティブにコンパイルされています。

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

次のJavaScriptは、TitleコンポーネントにExampleスタイルが適用されている場合、現在のページの最終変更日時をタイトルテキストの下に収集して挿入します。

jQueryの使用はオプションであり、使用する命名規則もオプションです。

次に、にある例のスタイルの[!DNL LESS]定義を示します。

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

* HTML（HTLを介して生成）は、できるだけ構造的に意味的である必要があります。不要な要素のグループ化やネストを避ける。
* HTML要素は、BEMスタイルのCSSクラスを使用してアドレス指定可能である必要があります。

**良好**  — コンポーネント内のすべての要素はBEM表記でアドレス指定可能です。

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**不良** -リスト要素とリスト要素は、要素名によってのみアドレス可能です。

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* データを公開して非表示にする方が、将来のバックエンド開発に必要なデータをあまりに少なくして公開するよりも、多くのデータを公開して非表示にする方が効果的です。

   * 作成者可能なコンテンツの切り替えを実装すると、このHTMLをリーンに保つのに役立ちます。これにより、作成者はHTMLに書き込むコンテンツ要素を選択できます。 は、特にHTMLに画像を書き込む際に重要になる場合があります。この場合、すべてのスタイルで使用されるとは限りません。
   * 例外は、高価なリソース（画像など）が初期設定で公開される場合です。CSSによって非表示にされるイベント画像は、この場合、不必要に取り込まれるので、このルールは例外です。

      * 最新の画像コンポーネントは、多くの場合、JavaScriptを使用してユースケース（ビューポート）に最も適した画像を選択して読み込みます。

### CSSのベストプラクティス{#css-best-practices}

>[!NOTE]
>
>スタイルシステムは、[BEM](https://en.bem.info/)の規定に従い、`BLOCK`と`BLOCK--MODIFIER`を同じ要素に適用しないという点で、[BEM](https://en.bem.info/)とは少し異なる技術的な相違を示す。
>
>代わりに、製品の制約により、`BLOCK--MODIFIER`が`BLOCK`要素の親に適用されます。
>
>[BEM](https://en.bem.info/)の他のテナントは、すべてとアラインする必要があります。

* CSSの定義を明確にし、再利用を可能にするには、[LESS](https://lesscss.org/)(AEMでネイティブにサポート)や[SCSS](https://sass-lang.com/)（カスタムビルドシステムが必要）などのプリプロセッサを使用します。

* セレクターの重み付け/特異性を一定に保つ。これにより、識別しにくいCSSカスケードの競合を回避し、解決できます。
* 各スタイルを個別のファイルに編成します。
   * これらのファイルは、LESS/SCSS `@imports`を使用して、または生のCSSが必要な場合、HTMLクライアントライブラリファイルを含めるか、カスタムフロントエンドアセットビルドシステムを使用して組み合わせることができます。
* 複雑なスタイルを多く混在させない。
   * 1つのコンポーネントに1度に適用できるスタイルが多いほど、多様な順列が作成されます。 これは、ブランドの位置づけを維持/QA/確実に行うのが難しくなる可能性があります。
* 常にCSSクラス（BEM表記に従う）を使用してCSSルールを定義します。
   * CSSクラスを持たない要素（ベア要素）を選択する必要が絶対にある場合は、CSS定義を高くして、選択可能なCSSクラスを持つそのタイプの要素との衝突よりも特異性が低いことを明確にします。
* `BLOCK--MODIFIER`はレスポンシブグリッドに付属しているので、直接スタイル設定を避けてください。 この要素の表示を変更すると、レスポンシブグリッドのレンダリングと機能に影響する可能性があるので、レスポンシブグリッドの動作を変更する意図がある場合は、このレベルのスタイルのみに影響します。
* `BLOCK--MODIFIER`を使用してスタイル範囲を適用します。 `BLOCK__ELEMENT--MODIFIERS`はコンポーネントで使用できますが、`BLOCK`はコンポーネントを表し、コンポーネントはスタイル設定されているので、スタイルは「定義済み」で、`BLOCK--MODIFIER`を介してスコープされます。

CSSセレクターの構造の例を次に示します。

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1レベルセレクタ</p> <p>BLOCK—MODIFIER</p> </td> 
   <td valign="bottom"><p>第2レベルセレクタ</p> <p>ブロック</p> </td> 
   <td valign="bottom"><p>第3レベルセレクター</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有効なCSSセレクタ</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-リスト — 暗</span></td> 
   <td valign="middle"><span class="code">.cmp-リスト</span></td> 
   <td valign="middle"><span class="code">.cmp-リスト__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-リスト — 暗</span></p> <p><span class="code"> .cmp-リスト</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-リスト__item {  </span></strong></p> <p><strong> color:青、</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color:赤、</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

ネストされたコンポーネントの場合、これらのネストされたコンポーネント要素のCSSセレクターの深さは、第3レベルのセレクターを超えます。 親コンポーネントの`BLOCK`でスコープされているネストされたコンポーネントに対して同じパターンを繰り返します。 つまり、ネストされたコンポーネントの3番目のレベルの`BLOCK`を開始し、ネストされたコンポーネントの`ELEMENT`を4番目のセレクターレベルに配置します。

### JavaScriptのベストプラクティス{#javascript-best-practices}

この節で定義するベストプラクティスは、「style-JavaScript」またはJavaScriptで、機能的な目的ではなく、スタイルのためにコンポーネントを操作することを目的としています。

* Style-JavaScriptは慎重に使用する必要があり、少数派の使用例です。
* Style-JavaScriptは主に、CSSによるスタイル設定をサポートするようにコンポーネントのDOMを操作するために使用します。
* コンポーネントがページ上に何度も表示される場合はJavaScriptの使用を再評価し、計算/再描画のコストを把握します。
* ページにコンポーネントが何度も表示される場合に、新しいデータ/コンテンツを非同期(AJAX経由)で取り込む場合、JavaScriptの使用を再評価します。
* PublishエクスペリエンスとAuthoringエクスペリエンスの両方を処理します。
* 可能な場合は、style-Javascriptを再使用します。
   * 例えば、1つのコンポーネントの複数のスタイルで、そのコンポーネントの画像を背景の画像に移動する必要がある場合、style-JavaScriptは1回だけ実装し、複数の`BLOCK--MODIFIERs`にアタッチできます。
* 可能な場合は、style-JavaScriptとfunctional JavaScriptを区切ります。
* これらのDOM変更をHTML内でHTL経由で直接表示する場合と比較して、JavaScriptのコストを評価します。
   * スタイルJavaScriptを使用するコンポーネントでサーバー側の変更が必要な場合は、JavaScriptの操作が現時点で可能かどうかを評価し、コンポーネントのパフォーマンスとサポートにどのような影響や影響があるかを評価します。

#### パフォーマンスに関する考慮事項{#performance-considerations}

* Style-JavaScriptは明確で薄い状態に保つ必要があります。
* ちらつきや不要な再描画を避けるには、まず`BLOCK--MODIFIER BLOCK`を介してコンポーネントを非表示にし、JavaScriptでのすべてのDOM操作が完了したら表示します。
* スタイルとJavaScriptの操作のパフォーマンスは、DOMReady上の要素を接続および変更する基本的なjQueryプラグインと同様です。
* 要求がgzipされ、CSSとJavaScriptが縮小されていることを確認します。

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEMクライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM （ブロック・エレメント・モディファイア）ドキュメントのWebサイト](https://getbem.com/)
* [LESSドキュメントWebサイト](https://lesscss.org/)
* [jQuery Webサイト](https://jquery.com/)
