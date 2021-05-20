---
title: クライアントサイドライブラリとフロントエンドワークフロー
description: Adobe Experience Manager(AEM)Sites実装のCSSとJavaScriptをデプロイおよび管理するために、クライアント側ライブラリ(clientlib)を使用する方法について説明します。 このチュートリアルでは、WebPackプロジェクトであるui.frontendモジュールをエンドツーエンドのビルドプロセスに統合する方法についても説明します。
sub-product: サイト
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: コアコンポーネント、AEM プロジェクトアーキタイプ
topic: コンテンツ管理、開発
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 10%

---


# クライアントサイドライブラリとフロントエンドワークフロー{#client-side-libraries}

Adobe Experience Manager(AEM)Sites実装のCSSとJavaScriptをデプロイおよび管理するために、クライアント側ライブラリ(clientlib)を使用する方法について説明します。 このチュートリアルでは、[ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html)モジュール（デカップ[webpack](https://webpack.js.org/)プロジェクト）をエンドツーエンドのビルドプロセスに統合する方法についても説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

また、クライアント側ライブラリとAEMの基本を理解するために、『[コンポーネントの基本](component-basics.md#client-side-libraries)』チュートリアルを確認することをお勧めします。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルで構築するベースラインコードを確認します。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の`tutorial/client-side-libraries-start`ブランチを確認します。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Mavenのスキルを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5または6.4を使用している場合は、任意のMavenコマンドに`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)で完成したコードをいつでも表示したり、ブランチ`tutorial/client-side-libraries-solution`に切り替えてコードをローカルでチェックアウトしたりできます。

## 目的

1. 編集可能なテンプレートを使用してクライアント側ライブラリをページに組み込む方法を理解します。
1. 専用のフロントエンド開発用に、UI.Frontend ModuleとWebpack開発サーバーを使用する方法を説明します。
1. コンパイル済みCSSとJavaScriptをSites実装に配信するエンドツーエンドのワークフローを理解します。

## 作成する内容 {#what-you-will-build}

この章では、実装を[UIデザインのモックアップ](assets/pages-templates/wknd-article-design.xd)に近づけるために、WKNDサイトと記事ページテンプレートのベースラインスタイルを追加します。 高度なフロントエンドワークフローを使用して、WebpackプロジェクトをAEMクライアントライブラリに統合します。

![完成したスタイル](assets/client-side-libraries/finished-styles.png)

*ベースラインスタイルが適用された記事ページ*

## 背景 {#background}

クライアント側ライブラリは、AEM Sites の実装で必要な CSS および JavaScript ファイルの編成および管理のための仕組みを提供します。クライアント側ライブラリまたはclientlibの基本的な目標は次のとおりです。

1. CSS／JS を、開発および管理が簡単な個別の小さなファイルに保存する
1. 組織立った方法で、サードパーティのフレームワークへの依存関係を管理する
1. CSS／JS を 1～2 個の要求に連結することで、クライアント側の要求数を最小限にする。

クライアント側ライブラリの使用の詳細については、[こちら](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html)を参照してください。

クライアント側ライブラリにはいくつかの制限があります。 最も顕著なのは、Sass、LESS、TypeScriptなどの一般的なフロントエンド言語に対する制限付きサポートです。 このチュートリアルでは、**ui.frontend**&#x200B;モジュールがこの問題の解決にどのように役立つかを見ていきます。

スターターコードベースをローカルAEMインスタンスにデプロイし、[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)に移動します。 このページは現在スタイル設定されていません。 次に、WKNDブランドのクライアント側ライブラリを実装し、ページにCSSとJavaScriptを追加します。

## クライアント側ライブラリ組織 {#organization}

次に、[AEMプロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/developing/archetype/overview.html)で生成されたclientlibの組織を調べます。

![クライアントライブラリ組織の概要](./assets/client-side-libraries/high-level-clientlib-organization.png)

*クライアント側ライブラリの組織とページを含む概要図*

>[!NOTE]
>
> 次のクライアント側ライブラリ組織は、AEM Project Archetypeによって生成されますが、出発点にすぎません。 プロジェクトがCSSとJavaScriptを最終的に管理してSites実装に配信する方法は、リソース、スキルセット、要件によって大きく異なります。

1. VSCodeまたは他のIDEを使用して、**ui.apps**&#x200B;モジュールを開きます。
1. パス`/apps/wknd/clientlibs`を展開して、アーキタイプで生成されたclientlibを表示します。

   ![ui.appsのclientlib](assets/client-side-libraries/four-clientlib-folders.png)

   これらのclientlibを以下で詳しく調べます。

1. 次の表に、クライアントライブラリの概要を示します。 クライアントライブラリを含む[の詳細は、](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)を参照してください。

   | 名前 | 説明 | 備考 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKNDサイトが機能するために必要なCSSとJavaScriptの基本レベル | コアコンポーネントのクライアントライブラリを埋め込む |
   | `clientlib-grid` | [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)が動作するために必要なCSSを生成します。 | モバイル/タブレットのブレークポイントは、ここで設定できます。 |
   | `clientlib-site` | WKNDサイト用のサイト固有のテーマを含む | `ui.frontend`モジュールによって生成されます |
   | `clientlib-dependencies` | サードパーティの依存関係を埋め込む | `ui.frontend`モジュールによって生成されます |

1. `clientlib-site`と`clientlib-dependencies`がソース管理から無視されることを確認します。 これは設計により、ビルド時に`ui.frontend`モジュールで生成されるので、これは設計によるものです。

## 基本スタイルを更新{#base-styles}

次に、**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;モジュールで定義されたベーススタイルを更新します。 `ui.frontend`モジュール内のファイルは、Siteテーマとサードパーティの依存関係を含む`clientlib-site`ライブラリと`clientlib-dependecies`ライブラリを生成します。

クライアント側ライブラリには、[Sass](https://sass-lang.com/)や[TypeScript](https://www.typescriptlang.org/)などの言語のサポートに関して、いくつかの制限があります。 フロントエンド開発を高速化および最適化するための、[NPM](https://www.npmjs.com/)や[webpack](https://webpack.js.org/)など、多数のオープンソースツールがあります。 **ui.frontend**&#x200B;モジュールの目的は、これらのツールを使用して、ほとんどのフロントエンドソースファイルを管理できることです。

1. **ui.frontend**&#x200B;モジュールを開き、`src/main/webpack/site`に移動します。
1. ファイル`main.scss`を開きます。

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` は、モジュール内のすべてのSassファイルへのエントリポイン `ui.frontend` トです。`_variables.scss`ファイルが含まれます。このファイルには、プロジェクト内の様々なSassファイルで使用される一連のブランド変数が含まれます。 `_base.scss`ファイルも含まれ、HTML要素の基本的なスタイルを定義します。 正規表現には、`src/main/webpack/components`の下にある個々のコンポーネントスタイルのすべてのスタイルが含まれます。 別の正規表現には、`src/main/webpack/site/styles`の下にあるすべてのファイルが含まれます。

1. `main.ts` ファイルを検査します。`main.ts` に `main.scss` は、プロジェクト内のファイルやファイルを収集す `.js` る正 `.ts` 規表現が含まれます。このエントリポイントは、[webpack設定ファイル](https://webpack.js.org/configuration/)によって`ui.frontend`モジュール全体のエントリポイントとして使用されます。

1. Inspect `src/main/webpack/site/styles`の下のファイル：

   ![スタイルファイル](assets/client-side-libraries/style-files.png)

   これらのファイルは、ヘッダー、フッター、メインコンテンツコンテナなど、テンプレート内のグローバル要素のスタイルを設定します。 これらのファイルのCSSルールの対象は、異なるHTML要素`header`、`main`および`footer`です。 これらのHTML要素は、前の章[ページとテンプレート](./pages-templates.md)のポリシーによって定義されています。

1. `src/main/webpack`の下の`components`フォルダーを展開し、ファイルを検査します。

   ![コンポーネントSassファイル](assets/client-side-libraries/component-sass-files.png)

   各ファイルは、[アコーディオンコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)などのコアコンポーネントにマッピングされます。 各コアコンポーネントは、[ブロック要素修飾子](https://getbem.com/)またはBEM表記を使用して構築され、スタイルルールで特定のCSSクラスを容易にターゲット設定できます。 `/components`の下のファイルは、AEMプロジェクトアーキタイプによって、コンポーネントごとに異なるBEMルールでスタブ化されています。

1. WKND基本スタイル&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)**&#x200B;と&#x200B;**unzip**&#x200B;をファイルにダウンロードします。

   ![WKNDベーススタイル](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   チュートリアルを高速化するために、コアコンポーネントと記事ページテンプレートの構造に基づくWKNDブランドを実装するSassファイルをいくつか用意しました。

1. `ui.frontend/src`の内容を前の手順のファイルで上書きします。 zipの内容によって次のフォルダーが上書きされます。

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![変更されたファイル](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect WKNDスタイルの実装の詳細を確認するために変更されたファイル。

## Inspect ui.frontend統合{#ui-frontend-integration}

**ui.frontend**&#x200B;モジュール[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)に組み込まれた主な統合要素は、コンパイル済みのCSSおよびJSアーティファクトをwebpack/npmプロジェクトから取り出し、AEMクライアント側ライブラリに変換します。

![ui.frontendアーキテクチャの統合](assets/client-side-libraries/ui-frontend-architecture.png)

AEMプロジェクトアーキタイプは、この統合を自動的に設定します。 次に、その仕組みを確認します。


1. コマンドラインターミナルを開き、`npm install`コマンドを使用して&#x200B;**ui.frontend**&#x200B;モジュールをインストールします。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` プロジェクトの新しいクローンまたは生成後に1回だけ実行する必要があります。

1. 同じターミナルで、`npm run dev`コマンドを使用して&#x200B;**ui.frontend**&#x200B;モジュールを構築し、デプロイします。

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > 「でエラーが発生しました。/src/main/webpack/site/main.scss」と入力します。
   > これは通常、`npm install`の実行後に環境が変更されたために発生します。
   > `npm rebuild node-sass`を実行して問題を修正します。 これは、ローカル開発マシンにインストールされている`npm`のバージョンが、`aem-guides-wknd/pom.xml`ファイル内のMaven `frontend-maven-plugin`で使用されているバージョンと異なる場合に発生します。 この問題は、POMファイル内のバージョンをローカルバージョンに合わせて変更するか、ローカルバージョンと完全に一致させることで修正できます。

1. コマンド`npm run dev`は、Webpackプロジェクトのソースコードを構築してコンパイルし、最終的に&#x200B;**clientlib-site**&#x200B;と&#x200B;**clientlib-dependencies**&#x200B;を&#x200B;**ui.apps**&#x200B;モジュールに設定する必要があります。

   >[!NOTE]
   >
   >また、JSとCSSを縮小する`npm run prod`プロファイルもあります。 これは、WebpackビルドがMaven経由でトリガーされる場合は常に、標準コンパイルです。 [ui.frontendモジュールの詳細は、](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)を参照してください。

1. `ui.frontend/dist/clientlib-site/css/site.css`の下の`site.css`ファイルをInspectします。 これは、Sassソースファイルに基づくコンパイル済みCSSです。

   ![分散サイトCSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` ファイルを検査します。これは、npmプラグイン[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)の設定ファイルで、`/dist`の内容をクライアントライブラリに変換し、`ui.apps`モジュールに移動します。

1. `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`の&#x200B;**ui.apps**&#x200B;モジュールの`site.css`ファイルをInspectします。 これは、**ui.frontend**&#x200B;モジュールの`site.css`ファイルの同じコピーである必要があります。 これで、**ui.apps**&#x200B;モジュールに配置され、AEMにデプロイできます。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site**&#x200B;は、ビルド時に&#x200B;**npm**&#x200B;または&#x200B;**maven**&#x200B;を使用してコンパイルされるので、**ui.apps**&#x200B;モジュールのソース管理から安全に無視できます。 **ui.apps**&#x200B;の下の`.gitignore`ファイルをInspectします。

1. 開発者ツールまたはMavenのスキルを使用して、`clientlib-site`ライブラリをAEMのローカルインスタンスと同期します。

   ![clientlibサイトの同期](assets/client-side-libraries/sync-clientlib-site.png)

1. AEMのLA Skatepark記事を開きます。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![記事のベーススタイルの更新](assets/client-side-libraries/updated-base-styles.png)

   これで、記事の更新されたスタイルが表示されます。 ブラウザーでキャッシュされたCSSファイルをクリアするには、ハードリフレッシュを実行する必要がある場合があります。

   モックアップの方がずっと近づいてきた！

   >[!NOTE]
   >
   > 上記の手順で、ui.frontendコードを構築してAEMにデプロイすると、Mavenビルドがプロジェクトのルート(`mvn clean install -PautoInstallSinglePackage`)からトリガーされたときに、自動的に実行されます。

>[!CAUTION]
>
> **ui.frontend**&#x200B;モジュールの使用は、すべてのプロジェクトで必要とは限りません。 **ui.frontend**&#x200B;モジュールは、さらに複雑さを増し、これらの高度なフロントエンドツール（Sass、webpack、npmなど）を使用する必要がない場合は、不要な場合があります。

## ページとテンプレートのインクルージョン{#page-inclusion}

次に、clientlibがAEMページでどのように参照されているかを確認します。 Web開発の一般的なベストプラクティスは、`</body>`タグを閉じる直前にHTMLヘッダー`<head>`とJavaScriptにCSSを含めることです。

1. **ui.apps**&#x200B;モジュールで、`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`に移動します。

   ![構造ページコンポーネント](assets/client-side-libraries/customheaderlibs-html.png)

   これは、WKND実装のすべてのページをレンダリングするために使用される`page`コンポーネントです。

1. `customheaderlibs.html` ファイルを開きます。行`${clientlib.css @ categories='wknd.base'}`に注意してください。 これは、カテゴリが`wknd.base`のクライアントライブラリのCSSが、このファイルを通じてインクルードされることを示しています。事実上、**clientlib-base**&#x200B;をすべてのページのヘッダーに含めます。

1. `customheaderlibs.html`を更新して、**ui.frontend**&#x200B;モジュールで前に指定したGoogleフォントスタイルへの参照を含めます。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. `customfooterlibs.html` ファイルを検査します。`customheaderlibs.html`のようなこのファイルは、プロジェクトを実装することで上書きされます。 ここで`${clientlib.js @ categories='wknd.base'}`という行は、**clientlib-base**&#x200B;のJavaScriptがすべてのページの末尾に含まれることを意味します。

1. 開発者ツールまたはMavenのスキルを使用して、`page`コンポーネントをAEMサーバーに書き出します。

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)にある記事ページテンプレートを参照します。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**ページポリシー**」を選択して、**ページポリシー**&#x200B;ダイアログを開きます。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy.png)

   *ページ情報/ページポリシー*

1. `wknd.dependencies`と`wknd.site`のカテゴリがここに表示されます。 デフォルトでは、ページポリシーを介して設定されたclientlibは分割され、ページの先頭にCSSを、本文の末尾にJavaScriptを含めます。 必要に応じて、clientlib JavaScriptがページの先頭に読み込まれることを明示的にリストできます。 これは`wknd.dependencies`の場合です。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > `wknd.base` clientlibの前述のように、 `customheaderlibs.html`または`customfooterlibs.html`スクリプトを使用して、ページコンポーネントから`wknd.site`または`wknd.dependencies`を直接参照することもできます。 テンプレートを使用すると、テンプレートごとに使用するclientlibを柔軟に選択できます。 例えば、非常に重いJavaScriptライブラリがあり、選択したテンプレートでのみ使用される場合などです。

1. **記事ページテンプレート**&#x200B;を使用して作成した&#x200B;**LA Skateparks**&#x200B;ページに移動します。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). フォントに違いがあるはずです。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**公開済みとして表示**」を選択して、AEMエディターの外部で記事ページを開きます。

   ![公開済みとして表示](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)のページソースを表示すると、`<head>`に次のclientlib参照が表示されます。

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   clientlibsはプロキシ`/etc.clientlibs`エンドポイントを使用しています。 また、次のクライアントライブラリインクルードがページの下部に表示されます。

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4に従うと、クライアント側ライブラリは自動的に縮小されません。 [HTMLライブラリマネージャーのドキュメントを参照して、最小化（推奨）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)を有効にしてください。

   >[!WARNING]
   >
   >セキュリティ上の理由から、/apps パスは&#x200B;**** Dispatcher の filter セクション&#x200B;**にのみ使用すべきであるので、パブリッシュ側では、クライアントライブラリを /apps から提供**&#x200B;しない[](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)ことが重要となります。クライアントライブラリの[allowProxyプロパティ](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)は、CSSとJSを&#x200B;**/etc.clientlibs**&#x200B;から提供するようにします。

## Webpack DevServer — 静的マークアップ{#webpack-dev-static}

前の2つの演習では、 **ui.frontend**&#x200B;モジュール内の複数のSassファイルを更新し、ビルドプロセスを通じて、最終的にこれらの変更がAEMに反映されていることを確認しました。 次に、[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)を活用して、**静的** HTMLに対するフロントエンドスタイルを迅速に開発する手法を見てみます。

この手法は、スタイルとフロントエンドコードの大部分が、AEM環境に容易にアクセスできない専用のフロントエンド開発者によって実行される場合に便利です。 また、この手法を使用すると、FEDはHTMLに直接変更を加え、それをAEM開発者に渡して、コンポーネントとして実装できます。

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)にあるLAスケートパークの記事ページのページソースをコピーします。
1. IDEを再度開きます。 AEMからコピーしたマークアップを、`src/main/webpack/static`の下の&#x200B;**ui.frontend**&#x200B;モジュールの`index.html`に貼り付けます。
1. コピーしたマークアップを編集し、**clientlib-site**&#x200B;と&#x200B;**clientlib-dependencies**&#x200B;への参照を削除します。

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   webpack開発サーバーはこれらのアーティファクトを自動的に生成するので、これらの参照を削除できます。

1. **ui.frontend**&#x200B;モジュール内から次のコマンドを実行して、新しいターミナルからwebpack開発サーバーを起動します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. これにより、静的なマークアップを使用して[http://localhost:8080/](http://localhost:8080/)に新しいブラウザウィンドウが開きます。

1. ファイル`src/main/webpack/site/_variables.scss`を編集します。 `$text-color`ルールを次のように置き換えます。

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   変更内容を保存します。

1. 変更が自動的に[http://localhost:8080](http://localhost:8080)のブラウザーに反映されます。

   ![ローカルwebpack開発サーバーの変更](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js`ファイルを確認します。 webpack-dev-serverの起動に使用するWebpack設定が含まれます。 AEMのローカルで実行されているインスタンスから、パス`/content`と`/etc.clientlibs`をプロキシします。 この方法で、画像や他のclientlib（**ui.frontend**&#x200B;コードで管理されていない）が使用可能になります。

   >[!CAUTION]
   >
   > 静的マークアップの画像srcは、ローカルのAEMインスタンス上のライブ画像コンポーネントを指します。 画像へのパスが変更された場合、AEMが開始されていない場合、またはブラウザーがローカルのAEMインスタンスにログインしていない場合、画像は壊れた状態で表示されます。 外部リソースに渡す場合は、画像を静的参照で置き換えることもできます。

1. コマンドラインから&#x200B;**stop**&#x200B;と入力して、WebPackサーバーを停止します。`CTRL+C`

## Webpack DevServer — 監視とaemsync {#webpack-dev-watch}

もう1つの方法は、`ui.frontend`モジュール内のsrcファイルに対するファイルの変更をNode.jsに監視させることです。 ファイルが変更されるたびに、クライアントライブラリがすばやくコンパイルされ、[aemsync](https://www.npmjs.com/package/aemsync) npmモジュールを使用して、実行中のAEMサーバーに変更を同期します。

1. **ui.frontend**&#x200B;モジュール内から次のコマンドを実行して、新しいターミナルから&#x200B;**watch**&#x200B;モードでwebpack開発サーバーを起動します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. これにより、`src`ファイルがコンパイルされ、変更内容がAEM([http://localhost:4502](http://localhost:4502))と同期されます。

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. AEMとLA Skateparksの記事に移動します。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![AEMにデプロイされた変更](assets/client-side-libraries/changes-deployed-aem-watch.png)

   変更をAEMにデプロイする必要があります。 若干の遅延が生じ、更新を確認するには、ブラウザーを手動で更新する必要があります。 ただし、新しいコンポーネントやダイアログのオーサリングを使用する場合は、AEMで直接変更を確認すると便利です。

1. 変更を`_variables.scss`に戻し、変更を保存します。 少し遅れてから、変更をAEMのローカルインスタンスと再び同期する必要があります。

1. webpack開発サーバーを停止し、プロジェクトのルートから完全なMavenビルドを実行します。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   繰り返しますが、 `ui.frontend`モジュールはコンパイルされ、クライアントライブラリに変換され、 `ui.apps`モジュールを介してAEMにデプロイされます。 しかし、今回はMavenがすべてを実行します。

## バリデーターが {#congratulations}

これで、記事ページにWKNDブランドと一致する一貫したスタイルが追加され、**ui.frontend**&#x200B;モジュールに慣れました。

### 次の手順 {#next-steps}

Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法について説明します。 [スタイルシステムを使用し](style-system.md) た開発では、スタイルシステムを使用して、ブランド固有のCSSとテンプレートエディターの高度なポリシー設定でコアコンポーネントを拡張します。

[GitHub](https://github.com/adobe/aem-guides-wknd)で完成したコードを表示するか、Gitブラッチ`tutorial/client-side-libraries-solution`でコードをローカルに確認してデプロイします。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)リポジトリのクローンを作成します。
1. `tutorial/client-side-libraries-solution`ブランチを確認します。

## その他のツールとリソース{#additional-resources}

### aemfed{#develop-aemfed} 

[****](https://aemfed.io/) aemfedisは、フロントエンド開発の高速化に使用できるオープンソースのコマンドラインツールです。[aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://www.npmjs.com/package/browser-sync)、[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)によって動作します。

**aemfed**&#x200B;の上位レベルでは、**ui.apps**&#x200B;モジュール内のファイルの変更をリッスンし、実行中のAEMインスタンスに直接同期するように設計されています。 変更に基づき、ローカルブラウザーが自動更新されるので、フロントエンド開発がスピードアップします。また、Sling Log tracerと連携して、サーバー側のエラーを直接ターミナルに自動的に表示するように設計されています。

**ui.apps**&#x200B;モジュール内で多くの作業を行い、HTLスクリプトを変更し、カスタムコンポーネントを作成する場合、**aemfed**&#x200B;は非常に強力なツールとなります。 [完全なドキュメントは、こちらを参照してください](https://github.com/abmaonline/aemfed)。

### クライアント側ライブラリのデバッグ{#debugging-clientlibs}

**categories**&#x200B;と&#x200B;**embeds**&#x200B;の異なる方法では、複数のクライアントライブラリを組み込む場合、トラブルシューティングに煩雑な場合があります。 AEM はそのためにいくつかのツールを公開しています。最も重要なツールの1つは、**クライアントライブラリの再構築**&#x200B;です。これは、AEMがLESSファイルを再コンパイルし、CSSを生成するよう強制します。

* [**ライブラリのダンプ**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**出力のテスト**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — カテゴリに基づいて、clientlibインクルードの予想されるHTML出力を確認できます。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**ライブラリの依存関係の検証**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 見つからない依存関係や埋め込みカテゴリをハイライトします。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**クライアントライブラリの再ビルド**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![クライアントライブラリの再構築](assets/client-side-libraries/rebuild-clientlibs.png)
