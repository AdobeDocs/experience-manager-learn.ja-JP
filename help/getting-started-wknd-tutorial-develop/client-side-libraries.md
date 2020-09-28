---
title: クライアント側ライブラリとフロントエンドワークフロー
description: Adobe Experience Manager(AEM)サイト実装のCSSとJavaScriptをデプロイおよび管理するために、クライアント側ライブラリまたはclientlibを使用する方法について説明します。 このチュートリアルでは、WebPackプロジェクトであるui.frontendモジュールをエンドツーエンドのビルドプロセスに統合する方法についても説明します。
sub-product: サイト
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 13%

---


# クライアント側ライブラリとフロントエンドワークフロー {#client-side-libraries}

Adobe Experience Manager(AEM)サイト実装のCSSとJavaScriptをデプロイおよび管理するために、クライアント側ライブラリまたはclientlibを使用する方法について説明します。 このチュートリアルでは、 [ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html) モジュール(非結合の [webpack](https://webpack.js.org/) プロジェクト)をエンドツーエンドのビルドプロセスに統合する方法についても説明します。

## 前提条件 {#prerequisites}

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

クライアント側ライブラリとAEMの基本事項を理解するため、 [コンポーネントの基本](component-basics.md#client-side-libraries) （英語）のチュートリアルも参照することをお勧めします。

### スタータープロジェクト

チュートリアルが構築する基本行コードを調べます。

1. github.com/adobe/aem-guides-wknd [リポジトリをコピーします](https://github.com/adobe/aem-guides-wknd) 。
1. ブランチをチェックアウトし `client-side-libraries/start` ます

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Mavenのスキルを使用して、ローカルAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `client-side-libraries/solution`ます。

## 目的

1. 編集可能なテンプレートを使用して、クライアントサイドライブラリをページに含める方法を理解します。
1. UI.Frontend ModuleとWebPack開発サーバーを使用して、専用のフロントエンド開発を行う方法を説明します。
1. コンパイル済みCSSとJavaScriptをSites実装に配信する、エンドツーエンドのワークフローを理解します。

## 作成する内容 {#what-you-will-build}

この章では、実装を [UIデザインのモックアップに近づけるために、WKNDサイトと記事ページテンプレートに基本スタイルを追加します](assets/pages-templates/wknd-article-design.xd)。 高度なフロントエンドワークフローを使用して、WebPackプロジェクトをAEMクライアントライブラリに統合します。

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## 背景 {#background}

クライアント側ライブラリは、AEM Sites の実装で必要な CSS および JavaScript ファイルの編成および管理のための仕組みを提供します。クライアント側のライブラリまたはclientlibの基本的な目標は次のとおりです。

1. CSS／JS を、開発および管理が簡単な個別の小さなファイルに保存する
1. 組織立った方法で、サードパーティのフレームワークへの依存関係を管理する
1. CSS／JS を 1～2 個の要求に連結することで、クライアント側の要求数を最小限にする。

クライアント側ライブラリの使用の詳細については、[こちら](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html)を参照してください。

クライアント側のライブラリにはいくつかの制限があります。 特に顕著なのは、Sass、LESS、TypeScriptなどの一般的なフロントエンド言語に対する限定的なサポートです。 このチュートリアルでは、 **ui.frontend** モジュールがこの解決にどのように役立つかを見ていきます。

スターターコードベースをローカルのAEMインスタンスにデプロイし、http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 このページのスタイルは現在解除されています。 次に、WKNDブランド用のクライアント側ライブラリを実装し、ページにCSSとJavaScriptを追加します。

## クライアント側ライブラリの組織 {#organization}

次に、 [AEM Project Archetypeで生成されたclientlibの組織を調べます](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html)。

![高レベルのクライアントライブラリ組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*ハイレベルな図クライアント側ライブラリの構成とページの組み込み*

>[!NOTE]
>
> 以下のクライアント側ライブラリ組織は、AEM Project Archetypeによって生成されますが、単なる出発点に過ぎません。 プロジェクトがCSSとJavaScriptを最終的にどのように管理し、Sites実装に配信するかは、リソース、スキルセットおよび要件に応じて大きく異なります。

1. Eclipseまたは他のIDEを使用して **ui.apps** モジュールを開きます。
1. アーキタイプ `/apps/wknd/clientlibs` で生成されたclientlibへのパスを表示します。

   ![ui.appsのClientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   以下では、これらのclientlibを詳しく調べます。

1. Inspectの特性 `clientlibs/clientlib-base`。

   **clientlib-base** は、WKNDサイトが機能するために必要なCSSとJavaScriptの基本レベルを表します。 に設定され `categories` ているプロパティに注目してく `wknd.base`ださい。 `categories` はclientlibのタグ付けメカニズムで、これらを参照する方法です。

   プロパティと `embed` 値 `String[]` の数に注目してください。 この `embed` プロパティは、カテゴリに基づいて他のclientlibを埋め込みます。 **clientlib-base** には、必要なAEMコアコンポーネントのクライアントライブラリがすべて含まれます。 これには、カルーセル用のjavascriptやクイック検索コンポーネントなど、機能するアーティファクトが含まれます。 **clientlib-base** は、独自のCSSとJavascriptを含まず、他のクライアントライブラリを埋め込むだけです。 **clientlib-base** は、のカテゴリで **clientlib-grid** clientlibを埋め込み `wknd.grid`ます。

   プロパティがに設定されていることに注意して `allowProxy` く `true`ださい。 常にclientlibに設定することがベストプラクティス `allowProxy=true` です。 この `allowProxy` プロパティを使用すると、clientlibを下のアプリケーションコードと共に格納できます `/apps` が **、エンドユーザーに対してアプリケーションコードが公開されるのを防ぐために**`/etc.clientlibs` 、プリフィックスが付いたパスにclientlibを配信します。 More information about the [allowProxy property can be found here.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspectの特性 `clientlibs/clientlib-grid`。

   **clientlib-grid** は、AEM Sitesエディタで [](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/authoring/siteandpage/responsive-layout.html) レイアウトモードを使用する場合に必要なCSSを含めたり、生成したりします。 **clientlib-grid** は、に設定されたカテゴリを持ち、clientlib-baseを介 `wknd.grid` して **埋め込まれています**。

   グリッドは、様々な量の列とブレークポイントを使用するようにカスタマイズできます。 次に、生成されたデフォルトのブレークポイントを更新します。

1. Update the file `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   これにより、で設定したテンプレートブレークポイントに対応するようにブレークポイントが変更され `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`ます。

   このファイルは、グリッドを生成するためのカスタムミックスインを含む `grid_base.less` ファイルを実際に参照し `/libs` ていることに注意してください。

1. Inspectの不動産 `clientlibs/clientlib-site`。

   **clientlib-site** には、WKNDブランド用のサイト固有のスタイルがすべて含まれます。 のカテゴリをメモしておき `wknd.site`ます。 このclientlibを生成するCSSとJavaScriptは、実際には `ui.frontend` モジュール内で維持されます。 次に、この統合について検討します。

1. Inspectの不動産 `clientlibs/clientlib-dependencies`。

   **clientlib-dependenciesは** 、サードパーティの依存関係を埋め込むことを目的としています。 これは別のclientlibで、必要に応じてHTMLページの先頭に読み込むことができます。 のカテゴリをメモしておき `wknd.dependencies`ます。 このclientlibを生成するCSSとJavaScriptは、実際には `ui.frontend` モジュール内で維持されます。 この統合については、チュートリアルの後半で説明します。

## Using the ui.frontend module {#ui-frontend}

次に、 **[ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html)** モジュールの使用方法を説明します。

### 動機

SassやTypeScriptなどの言語をサポートする場合、クライアント側ライブラリにはいくつかの制限があり [ます](https://sass-lang.com/)[](https://www.typescriptlang.org/)。 また、 [NPM](https://www.npmjs.com/) 、 [Webpackなどのオープンソースツールが爆発的に増え、フロントエンドの開発を迅速化し最適化しています](https://webpack.js.org/) 。

ui.frontend **** モジュールの基本的な考え方は、NPMやWebpackなどの優れたツールを使用して、ほとんどのフロントエンド開発を管理できることです。 ui.frontend **モジュールに組み込まれている主な統合要素である** aem-clientlib-generatorは [](https://github.com/wcm-io-frontend/aem-clientlib-generator) 、webpack/npmプロジェクトからコンパイルされたCSSおよびJSアーティファクトを取得し、AEMクライアント側ライブラリに変換します。 これにより、フロントエンド開発者は、様々なツールやテクノロジーを選択する自由度が向上します。

![ui.frontendアーキテクチャ統合](assets/client-side-libraries/ui-frontend-architecture.png)

### 使用方法

次に、`.scss` ui.frontend **** モジュールを介してSassファイル（拡張子）をいくつか追加し、WKNDブランドの基本スタイルを追加します。

1. **ui.frontend** モジュールを開き、に移動し `src/main/webpack/base/sass`ます。

   ![ui.frontendモジュール](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. フォルダーの下に、という名前の新しいファイル `_variables.scss` を作成 `src/main/webpack/base/sass`します。
1. `_variables.scss` に以下を入力します。

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sassを使用すると、変数を作成できます。変数は異なるファイル全体で使用でき、一貫性を確保できます。 フォントファミリーに注目してください。 これらのフォントを使用するために、Google Webフォントへの呼び出しを含める方法について、チュートリアルの後半で説明します。

1. の下に別のファイルを作成 `_elements.scss` し、次 `src/main/webpack/base/sass` の内容を入力します。

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   この `_elements.scss` ファイルでは、の変数を使用し `_variables.scss`ます。

1. と `_shared.scss` ファイル `src/main/webpack/base/sass` を含めるには、の下に更新 `_elements.scss``_variables.scss` します。

   ```css
   @import './variables';
   @import './elements';
   ```

1. コマンドラインターミナルを開き、次の **コマンドを使用して** ui.frontend `npm install` モジュールをインストールします。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 新しいクローンまたはプロジェクトの生成の後、1回だけ実行する必要があります。

1. 同じターミナルで、次の **コマンドを使用して** ui.frontend `npm run dev` モジュールを構築し、デプロイします。

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   このコマンド `npm run dev` は、Webpackプロジェクトのソースコードを構築してコンパイルし、最終的には **ui.apps** モジュールのclientlib-site **と** clientlib-dependencies **** に設定する必要があります。

   >[!NOTE]
   >
   >また、JSとCSSを縮小する `npm run prod` プロファイルもあります。 Mavenを介してWebPackビルドがトリガーされるたびに、標準コンパイルが実行されます。 ui. [frontendモジュールの詳細については、こちらを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspectはその下のファイル `site.css` を見 `ui.frontend/dist/clientlib-site/css/site.css`た。 CSSは、主に前に作成した `_elements.scss` ファイルの内容で構成されていますが、変数は実際の値に置き換えられています。

   ![分散サイトCSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. ファイルをInspect `ui.frontend/clientlib.config.js`。 次に、npmプラグイン [aem-clientlib-generatorの設定ファイルを示し](https://github.com/wcm-io-frontend/aem-clientlib-generator)ます。 **aem-clientlib-generator** は、コンパイル済みのCSS/JavaScriptを変換し、 **ui.apps** モジュールにコピーするツールです。

1. ファイルのInspect処理( `site.css` ui.apps **モジュール)** を `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`参照してください。 これは、 `site.css` ui.frontend **** モジュールのファイルと同じコピーである必要があります。 これで、 **ui.apps** モジュールに追加され、AEMにデプロイできます。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > clientlib-site **は実際にビルド時にコンパイルされるので、** npm **または** mavenを使用しているので **、実際には****** ui.appsモジュールのソース管理から無視することができます。 ui.appsの下の `.gitignore` ファイルにInspect **を指定します**。

>[!CAUTION]
>
> すべてのプロジェクトで **ui.frontend** モジュールを使用する必要がない場合があります。 ui.frontend **** モジュールは複雑さを増し、これらの高度なフロントエンドツール（Sass、webpack、npmなど）の一部を使用する必要がない場合は、過剰に処理される可能性があります。 このため、AEMプロジェクトアーキタイプのオプションの一部と見なされ、標準のクライアント側ライブラリ、バニラCSSおよびJavaScriptの使用は引き続き完全にサポートされます。

## ページとテンプレートを含める {#page-inclusion}

次に、AEMのテンプレート/ページにclientlibを含めるようにプロジェクトがどのように設定されているかを確認します。 Web開発の一般的なベストプラクティスは、タグを閉じる直前に、CSSをHTMLヘッダー `<head>` とJavaScriptに含めること `</body>` です。

1. ui.apps **モジュールで** 、に移動し `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`ます。

   ![構造ページコンポーネント](assets/client-side-libraries/customheaderlibs-html.png)

   これは、WKND実装のすべてのページをレンダリングするために使用される `page` コンポーネントです。

1. Open the file `customheaderlibs.html`. 線に注目し `${clientlib.css @ categories='wknd.base'}`ます。 これは、のカテゴリを持つclientlibのCSSが、このファイルを通じて含ま `wknd.base` れることを示しています。このファイルは、 **clientlib-base** を全ページのヘッダに含めるのに効果的です。

1. 前 `customheaderlibs.html` の例で **ui.frontend** モジュールで指定したGoogleフォントスタイルへの参照を含めるように更新します。 ContextHubについてもコメントアウトします。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. ファイルをInspect `customfooterlibs.html`。 このファイルは、プロジェクト `customheaderlibs.html` の実装時に上書きするファイルと同様です。 この行は、 `${clientlib.js @ categories='wknd.base'}` clientlib-baseのJavaScriptがすべてのページの末尾に含まれるこ **** とを意味します。

1. Mavenを使用して、プロジェクトを構築し、ローカルのAEMインスタンスにデプロイします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wkndのWKNDテンプレートを参照し [ます](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)。

1. テンプレートエディターで **記事ページテンプレート** (Article Page Template)を選択して開きます。

   ![記事ページテンプレートを選択](assets/client-side-libraries/open-article-page-template.png)

1. 「 **ページ情報** 」アイコンをクリックし、メニューで「 **ページポリシー** 」を選択して、 **ページポリシー** ダイアログを開きます。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy.png)

   *ページ情報/ページポリシー*

1. とのカテゴリがここに表示さ `wknd.dependencies` れ `wknd.site` ています。 デフォルトでは、ページポリシーで設定したclientlibは分割され、ページのheadにCSSが含まれ、本文の末尾にJavaScriptが含まれます。 必要に応じて、ページのheadにclientlib JavaScriptが読み込まれることを明示的にリストできます。 これがその例で `wknd.dependencies`す。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > また、clientlibの前の説明で説明したように、 `wknd.site` または `wknd.dependencies` スクリプトを使用して、ページコンポーネントを直接参照す `customheaderlibs.html``customfooterlibs.html``wknd.base` ることも可能です。 テンプレートを使用すると、テンプレートごとに使用するclientlibを柔軟に選択できます。 例えば、非常に重いJavaScriptライブラリがあり、選択したテンプレートでのみ使用される場合、

1. 記事ページテンプレートを使用して作成した **LA Skateparks** ページに移動します ****。 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). フォントの違いと、 **ui.frontend** モジュールで作成されたCSSが機能していることを示すために適用される基本的なスタイルがいくつかあります。

1. 「 **ページ情報** 」アイコンをクリックし、メニューで「発行済みの **** 表示」を選択して、AEMエディターの外部で記事ページを開きます。

   ![公開済みとして表示](assets/client-side-libraries/view-as-published-article-page.png)

1. http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled [のページソースを表示し](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 、次のclientlib参照をに表示できるはずです `<head>`。

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   clientlibがプロキシ `/etc.clientlibs` エンドポイントを使用していることに注意してください。 また、次のclientlibインクルードがページの最下部に表示されます。

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >セキュリティ上の理由から、/apps パスは&#x200B;**** Dispatcher の filter セクション&#x200B;**にのみ使用すべきであるので、パブリッシュ側では、クライアントライブラリを /apps から提供**&#x200B;しない[](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)ことが重要となります。The [allowProxy property](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) of the client library ensures the CSS and JS are served from **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

前の2つの練習では、 **ui.frontend** モジュール内のいくつかのSassファイルを更新し、構築プロセスを通じて、最終的にこれらの変更がAEMに反映されていることを確認しました。 次に、 [webpack-dev-serverを活用して、フロントエンドスタイルを迅速に開発する方法を説明します](https://webpack.js.org/configuration/dev-server/) 。

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

次に、このビデオに示す高レベルの手順を示します。

1. 次のコマンドを **ui.frontend** モジュールから実行して、webpackデベロッパーサーバーを開始します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. これにより、静的マークアップの [あるhttp://localhost:8080/に新しいブラウザウィンドウが開きます](http://localhost:8080/) 。
1. LA skatepark記事ページ(http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)のページソースをコピーし [ます](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. AEMからコピーしたマークアップを、下の `index.html` ui.frontend **モジュールのに貼り付け**`src/main/webpack/static`ます。
1. コピーしたマークアップを編集し、 **clientlib-site** と **clientlib-dependenciesへの参照を削除します**。

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   これらの参照は削除できます。webpack開発サーバーによってこれらのアーティファクトが自動的に生成されるからです。

1. ファイルを編集し、変更内容が自動的にブラウザーに反映されることを確認します。 `.scss`
1. ファイルを確認し `/aem-guides-wknd.ui.frontend/webpack.dev.js` ます。 webpack-dev-serverの開始に使用するwebpack設定が含まれます。 AEMのローカルで実行されているインスタンス `/content` から、およびパス `/etc.clientlibs` をプロキシします。 これにより、画像や他のclientlib ( **ui.frontend** コードでは管理されない)が利用できるようになります。

   >[!CAUTION]
   >
   > 静的マークアップの画像srcは、ローカルAEMインスタンス上のライブ画像コンポーネントを指します。 画像へのパスが変更された場合、AEMが起動されていない場合、またはブラウザーが使用しているローカルのAEMインスタンスにログインしていない場合、画像は壊れて表示されます。
1. コマンドラインで **、webpackサーバーを** 停止するには、と入力し `CTRL+C`ます。

## まとめ {#putting-it-together}

このチュートリアルでは、クライアント側のライブラリと、AEMとの統合に必要な潜在的なフロントエンドワークフローに焦点を当てます。 これを念頭に置いて、 [](assets/client-side-libraries/client-side-libraries-final-styles.zip)client-side-libraries-final-styles.zipをインストールし、実装を高速化します。このファイルには、記事ページテンプレートで使用されるコアコンポーネントのいくつかのデフォルトのスタイルが含まれます。

* [パンくず](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/breadcrumb.html)
* [ダウンロード](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [画像](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html)
* [リスト](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/list.html)
* [ナビゲーション](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/navigation.html)
* [クイック検索](https://docs.adobe.com/content/help/jp/experience-manager-core-components/using/components/quick-search.html)
* [区切り文字](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

次に、このビデオに示す高レベルの手順を示します。

1. client-side-libraries-final-styles.zip [をダウンロードし](assets/client-side-libraries/client-side-libraries-final-styles.zip) 、下のコンテンツを解凍 `ui.frontend/src/main/webpack`します。 zipの内容によって次のフォルダーが上書きされます。

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. webpack devサーバーを使用して新しいスタイルをプレビューします。

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. コードベースをローカルAEMインスタンスにデプロイし、LA skate park記事に適用された新しいスタイルを確認します。

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## バリデーターが{#congratulations}

これで、記事ページにWKNDブランドと一致する一貫したスタイルがいくつか作成され、 **ui.frontend** モジュールに慣れました。

### 次の手順 {#next-steps}

Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法を説明します。 [スタイルシステムを使用した開発では](style-system.md) 、スタイルシステムを使用して、コアコンポーネントをブランド固有のCSSおよびテンプレートエディターの高度なポリシー設定で拡張する方法について説明します。

GitHubに完了したコードを表示する [か](https://github.com/adobe/aem-guides-wknd) 、コードをローカルのGitブラッチに確認してデプロイ `client-side-libraries/solution`します。

1. github.com/adobe/aem-wknd-guides [リポジトリをコピーします](https://github.com/adobe/aem-guides-wknd) 。
1. ブランチをチェックアウトし `client-side-libraries/solution` ます。

## その他のツールおよびリソース {#additional-resources}

### aemfed{#develop-aemfed} 

[**aemfed**](https://aemfed.io/) は、フロントエンドの開発を高速化するために使用できる、オープンソースのコマンドラインツールです。 Powered by [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) , and [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

高レベルの埋め込み **は** 、 **ui.apps** モジュール内のファイル変更をリッスンして、実行中のAEMインスタンスに自動的に同期するように設計されています。 変更に基づき、ローカルブラウザーが自動更新されるので、フロントエンド開発がスピードアップします。また、Slingログトレーサと連携して、サーバ側のエラーを端末に直接表示するように設計されています。

ui.apps **モジュール内で多くの作業を行う場合、HTLスクリプトの変更やカスタムコンポーネントの作成を行う場合、** aemfed **** は非常に強力なツールとして使用できます。 [完全なドキュメントは、こちらを参照してください。](https://github.com/abmaonline/aemfed).

### クライアント側ライブラリのデバッグ {#debugging-clientlibs}

With different methods of **categories** and **embeds** to include multiple client libraries it can be cumbersome to troubleshoot. AEM はそのためにいくつかのツールを公開しています。One of the most important tools is **Rebuild Client Libraries** which will force AEM to re-compile any LESS files and generate the CSS.

* [**Libsのダンプ**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**テスト出力**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) -カテゴリに基づいてclientlibインクルードのHTML出力が期待通りに表示されるようにします。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**ライブラリ依存関係の検証**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) — 見つからない依存関係や埋め込みカテゴリを強調表示します。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**クライアントライブラリの再ビルド**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![クライアントライブラリを再構築](assets/client-side-libraries/rebuild-clientlibs.png)
