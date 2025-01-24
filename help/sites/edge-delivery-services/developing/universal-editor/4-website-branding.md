---
title: Web サイトのブランディングの追加
description: Edge Delivery Servicesサイトのグローバル CSS、CSS 変数および web フォントを定義します。
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
source-wordcount: '1308'
ht-degree: 0%

---


# Web サイトのブランディングの追加

最初に、グローバルスタイルを更新し、CSS 変数を定義し、web フォントを追加して、ブランド全体を設定します。 これらの基本要素により、web サイトの一貫性と保守性が維持され、サイト全体で一貫して適用される必要があります。

## GitHub イシューの作成

すべてを整理しておくには、GitHub を使用して作業を追跡します。 まず、この作業体系の GitHub イシューを作成します。

1. GitHub リポジトリに移動します（詳しくは、[ コードプロジェクトの作成 ](./1-new-code-project.md) の章を参照）。
2. **イシュー** タブをクリックしてから、**新しいイシュー** をクリックします。
3. 作業を行うために、**タイトル** と **説明** を記述します。
4. **新しいイシューを送信** をクリックします。

GitHub イシューは、後で [ プルリクエストの作成 ](#merge-code-changes) 時に使用されます。

![GitHub 新しいイシューを作成 ](./assets/4-website-branding/github-issues.png)

## 作業ブランチの作成

組織を維持し、コードの品質を確保するには、作業の各部門に新しいブランチを作成します。 これにより、新しいコードがパフォーマンスに悪影響を与えるのを防ぎ、変更が完了する前に変更が有効にならないようにします。

この章では、web サイトの基本的なスタイルに重点を置いて、`wknd-styles` という名前のブランチを作成します。

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## グローバル CSS

Edge Delivery Servicesは、`styles/styles.css` にあるグローバル CSS ファイルを使用して、web サイト全体の共通スタイルを設定します。 `styles.css` では、色、フォント、間隔などの側面を制御し、サイト全体ですべてが一貫して見えるようにします。

グローバル CSS は、ブロックなどの下位レベルの構成に依存せず、サイトの全体的なルックアンドフィールや共有される視覚的処理に重点を置く必要があります。

グローバル CSS スタイルは、必要に応じて上書きできます。

### CSS 変数

[CSS 変数 ](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) は、カラー、フォント、サイズなどのデザイン設定を保存する優れた方法です。 変数を使用すると、これらの要素を 1 か所で変更し、サイト全体で更新できます。

CSS 変数のカスタマイズを開始するには、次の手順に従います。

1. コードエディターで `styles/styles.css` ファイルを開きます。
2. グローバル CSS 変数が格納されている `:root` 宣言を見つけます。
3. WKND ブランドに一致するようにカラーおよびフォント変数を変更します。

次に例を示します。


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

`:root` の節のその他の変数を探索し、デフォルト設定を確認します。

Web サイトを開発し、同じ CSS 値を繰り返す状況が発生した場合は、スタイルの管理を容易にするために新しい変数の作成を検討します。 CSS 変数を利用するその他の CSS プロパティの例としては、`border-radius`、`padding`、`margin`、`box-shadow` などがあります。

### ベア要素

ベア要素は、CSS クラスを使用する代わりに、要素名を通じて直接スタイル設定されます。 例えば、`.page-heading` の CSS クラスにスタイルを設定するのではなく、`h1 { ... }` を使用して `h1` 要素にスタイルが適用されます。

`styles/styles.css` ファイルでは、ベアHTML要素に一連のベーススタイルが適用されます。 Edge Delivery Services web サイトでは、Edge Delivery サービスのネイティブなセマンティックHTMLに合わせているので、ベア要素の使用が優先されます。

WKND ブランディングに合わせるには、`styles.css` でいくつかのベア要素のスタイルを設定します。

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

これらのスタイルにより、`h2` 要素は、上書きされない限り、WKND ブランディングを使用して一貫してスタイル設定され、明確な視覚階層を作成するのに役立ちます。 各 `h2` の下の部分的な黄色い下線は、見出しに独特のタッチを追加します。

### 推測される要素

Edge Delivery Servicesでは、プロジェクトの `scripts.js` と `aem.js` コードは、HTML内のコンテキストに基づいて、特定のベアHTML要素を自動的に強化します。

例えば、囲まれたテキストをインラインで囲むのではなく、独自の行で作成したアンカー（`<a>`）要素は、このコンテキストに基づいてボタンと推論されます。 これらのアンカーは、CSS クラス `button-container` を持つコンテナ `div` で自動的にラップされ、アンカー要素には `button` の CSS クラスが追加されます。

例えば、リンクが単独の行で作成される場合、Edge Delivery Services JavaScriptは DOM を次のように更新します。

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

これらのボタンは、WKND ブランドに合わせてカスタマイズできます。WKND ブランドでは、ボタンが黒いテキストの黄色の長方形として表示されます。

次に、`styles.css` で「推測されるボタン」のスタイルを設定する方法の例を示します。

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

この CSS は基本ボタンスタイルを定義し、大文字テキスト、黄色の背景、黒のテキストなど WKND 固有の処理が含まれます。 `background-color` プロパティと `color` プロパティでは、CSS 変数を使用して、ボタンのスタイルをブランドのカラーと揃えることができます。 このアプローチにより、柔軟に保ちながら、サイト全体でボタンのスタイルが一貫して設定されます。

## Web フォント

Edge Delivery Servicesプロジェクトでは、web フォントの使用を最適化して高いパフォーマンスを維持し、Lighthouse スコアへの影響を最小限に抑えます。 この方法では、サイトの視覚的なアイデンティティを損なうことなく高速なレンダリングが保証されます。 最適なパフォーマンスを得るために web フォントを効率的に実装する方法を以下に示します。

### 書体面

`styles/fonts.css` ファイルで CSS `@font-face` 宣言を使用してカスタム web フォントを追加します。 `fonts.css` に `@font-faces` を追加すると、最適な時間に web フォントが読み込まれるので、Lighthouse のスコアを維持するのに役立ちます。

1. `styles/fonts.css` を開きます。
2. WKND ブランドフォントを含めるには、`Asar` および `Source Sans Pro` の `@font-face` 宣言を追加します。

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

このチュートリアルで使用するフォントはGoogle Fonts から取得していますが、web フォントは、[Adobe Fonts](https://fonts.adobe.com/) を含むすべてのフォントプロバイダーから取得できます。

+++ローカル web フォントファイルの使用

または、Web フォントファイルを `/fonts` フォルダーのプロジェクトにコピーして、`@font-face` 宣言で参照できます。

このチュートリアルでは、リモートのホスト型 web フォントを使用するので、より簡単にフォローできます。

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

最後に、新しいフォントを使用するように `styles/styles.css` の CSS 変数を更新します。

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` と `roboto-condensed-fallback` は、カスタムフ `Asar` ームと `Source Sans Pro` の web フォントをサポートするために「[ フォールバックフォント ](#fallback-fonts)」セクションで更新され、連携するフォールバックフォントです。

### フォールバックフォント

Web フォントは、サイズが原因でパフォーマンスに影響を与えることが多く、累積レイアウトシフト（CLS）スコアが増加し、Lighthouse の全体的なスコアが低下する可能性があります。 Web フォントの読み込み中にテキストを即座に表示するために、Edge Delivery Servicesプロジェクトでは、ブラウザーネイティブのフォールバックフォントが使用されます。 このアプローチにより、目的のフォントを適用しながら、スムーズなユーザーエクスペリエンスを維持できます。

最適なフォールバックフォントを選択するには、Adobeの [Helix Font Fallback Chrome拡張機能 ](https://www.aem.live/developer/font-fallback) を使用します。この拡張機能により、カスタムフォントが読み込まれる前に、使用するブラウザーのフォントが厳密に一致するかを決定します。 結果として生成されるフォールバックフォント宣言は、パフォーマンスを向上させ、ユーザーにシームレスなエクスペリエンスを提供するために、`styles/styles.css` ファイルに追加する必要があります。

[Helix Font Fallback Chrome拡張機能 ](https://www.aem.live/developer/font-fallback) を使用するには、Edge Delivery Servicesの web サイトで使用されるのと同じバリエーションに web フォントが適用されている web ページがあることを確認します。 このチュートリアルでは、[wknd.site](http://wknd.site/us/en.html) の拡張機能について説明します。 Web サイトを開発するときは、[wknd.site](http://wknd.site/us/en.html) ではなく、作業中のサイトに拡張機能を適用します。

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

フォールバックフォントファミリー名を `styles/styles.css` のフォント CSS 変数の「実際の」フォントファミリー名の後に追加します。

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## 開発のプレビュー

CSS を追加すると、AEM CLI のローカル開発環境が変更内容を自動的に再読み込みするので、CSS がブロックに与える影響をすばやく簡単に確認できます。

![WKND ブランド CSS の開発プレビュー ](./assets/4-website-branding/preview.png)


## 最終的な CSS ファイルのダウンロード

更新された CSS ファイルは、以下のリンクからダウンロードできます。

* [`styles.css`](https://adobe.com#TODO)
* [`fonts.css`](https://adobe.com#TODO)

## CSS ファイルのリンク

コードの変更を [ 頻繁に lint](./3-local-development-environment.md#linting) し、クリーンで一貫性のあるものにします。 定期的にリンティングを行うと、問題を早期に発見でき、開発全体の時間を短縮できます。 リンティングの問題がすべて解決されるまで、作業を main ブランチに結合できないことに注意してください。

```bash
$ npm run lint:css
```

## コードの変更を結合

変更内容を GitHub の `main` ブランチに結合すると、これらの更新時に将来の作業を構築できます。

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

変更が `wknd-styles` ブランチにプッシュされたら、GitHub でプルリクエストを作成して、`main` ブランチにマージします。

1. [ プロジェクトの新規作成 ](./1-new-code-project.md) の章で作成した GitHub リポジトリに移動します。
2. 「**プルリクエスト**」タブをクリックし、「**新規プルリクエスト**」を選択します。
3. `wknd-styles` をソースブランチ、`main` をターゲットブランチとして設定します。
4. 変更を確認し、「**プルリクエストを作成**」をクリックします。
5. プルリクエストの詳細で、**次を追加します**。

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1` では、先ほど作成した GitHub の問題について説明しています。
   * テスト URL は、検証と比較に使用するブランチをAEM コード同期に示します。 「After」 URL は、作業ブランチ `wknd-styles` を使用して、コードの変更が Web サイトのパフォーマンスに与える影響を確認します。

6. **プルリクエストを作成** をクリックします。
7. [AEM コード同期 GitHub アプリ ](./1-new-code-project.md) が **品質チェックを完了** するまで待ちます。 失敗した場合は、エラーを解決してチェックを再実行します。
8. チェックが合格したら、**プルリクエストをマージ** して `main` に入れます。

変更内容を `main` にマージすると、実稼動環境にデプロイされたものとは見なされず、これらの更新に基づいて新しい開発を続行できます。
