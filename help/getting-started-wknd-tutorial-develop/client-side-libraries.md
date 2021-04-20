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
feature: コアコンポーネント、AEM プロジェクトアーキタイプ
topic: コンテンツ管理、開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 10%

---


# クライアント側ライブラリとフロントエンドワークフロー{#client-side-libraries}

Adobe Experience Manager(AEM)サイト実装のCSSとJavaScriptをデプロイおよび管理するために、クライアント側ライブラリまたはclientlibを使用する方法について説明します。 このチュートリアルでは、[ui.frontend](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/uifrontend.html)モジュール（デカップされた[webpack](https://webpack.js.org/)プロジェクト）をエンドツーエンドのビルドプロセスに統合する方法についても説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

また、[基本コンポーネント](component-basics.md#client-side-libraries)チュートリアルを確認して、クライアント側のライブラリとAEMの基本事項を理解することをお勧めします。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルが構築する基本行コードを調べます。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の`tutorial/client-side-libraries-start`ブランチを調べます。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Mavenのスキルを使用して、ローカルAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5または6.4を使用している場合は、Mavenコマンドに`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)に常に表示できます。また、ブランチ`tutorial/client-side-libraries-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 目的

1. 編集可能なテンプレートを使用して、クライアントサイドライブラリをページに含める方法を理解します。
1. UI.Frontend ModuleとWebPack開発サーバーを使用して、専用のフロントエンド開発を行う方法を説明します。
1. コンパイル済みCSSとJavaScriptをSites実装に配信する、エンドツーエンドのワークフローを理解します。

## 作成する内容 {#what-you-will-build}

この章では、実装を[UIデザインのモックアップ](assets/pages-templates/wknd-article-design.xd)に近づけるために、WKNDサイトと記事ページテンプレートに基本スタイルを追加します。 高度なフロントエンドワークフローを使用して、WebPackプロジェクトをAEMクライアントライブラリに統合します。

![仕上げスタイル](assets/client-side-libraries/finished-styles.png)

*ベースラインスタイルが適用された記事ページ*

## 背景 {#background}

クライアント側ライブラリは、AEM Sites の実装で必要な CSS および JavaScript ファイルの編成および管理のための仕組みを提供します。クライアント側のライブラリまたはclientlibの基本的な目標は次のとおりです。

1. CSS／JS を、開発および管理が簡単な個別の小さなファイルに保存する
1. 組織立った方法で、サードパーティのフレームワークへの依存関係を管理する
1. CSS／JS を 1～2 個の要求に連結することで、クライアント側の要求数を最小限にする。

クライアント側ライブラリの使用の詳細については、[こちら](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html)を参照してください。

クライアント側のライブラリにはいくつかの制限があります。 特に顕著なのは、Sass、LESS、TypeScriptなどの一般的なフロントエンド言語に対する限定的なサポートです。 このチュートリアルでは、**ui.frontend**&#x200B;モジュールがこの問題を解決する方法を見ていきます。

スターターコードベースをローカルAEMインスタンスにデプロイし、[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)に移動します。 このページのスタイルは現在解除されています。 次に、WKNDブランド用のクライアント側ライブラリを実装し、ページにCSSとJavaScriptを追加します。

## クライアント側ライブラリ組織{#organization}

次に、[AEMプロジェクトのアーキタイプ](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/developing/archetype/overview.html)で生成されたclientlibの組織を調べます。

![高レベルのクライアントライブラリ組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*ハイレベルな図クライアント側ライブラリの構成とページの組み込み*

>[!NOTE]
>
> 以下のクライアント側ライブラリ組織は、AEM Project Archetypeによって生成されますが、単なる出発点に過ぎません。 プロジェクトがCSSとJavaScriptを最終的にどのように管理し、Sites実装に配信するかは、リソース、スキルセットおよび要件に応じて大きく異なります。

1. VSCodeまたは他のIDEを使用して、**ui.apps**&#x200B;モジュールを開きます。
1. パス`/apps/wknd/clientlibs`を展開し、アーキタイプで生成されたclientlibを表示します。

   ![ui.appsのClientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   以下では、これらのclientlibを詳しく調べます。

1. 次の表に、クライアントライブラリの概要を示します。 クライアントライブラリを含む[の詳細は、](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)を参照してください。

   | 名前 | 説明 | 備考 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKNDサイトが機能するために必要なCSSとJavaScriptの基本レベル | コアコンポーネントのクライアントライブラリを埋め込む |
   | `clientlib-grid` | [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)が機能するのに必要なCSSを生成します。 | モバイル/タブレットのブレークポイントはここで設定できます |
   | `clientlib-site` | WKNDサイト用のサイト固有のテーマを含む | `ui.frontend`モジュールで生成 |
   | `clientlib-dependencies` | サードパーティの依存関係を埋め込みます | `ui.frontend`モジュールで生成 |

1. `clientlib-site`と`clientlib-dependencies`がソース管理から無視されることを確認してください。 これは設計上、`ui.frontend`モジュールがビルド時に生成するので、これは設計上のものです。

## 基本スタイルを更新{#base-styles}

次に、**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;モジュールで定義されている基本スタイルを更新します。 `ui.frontend`モジュール内のファイルは、Siteテーマとサードパーティの依存関係を含む`clientlib-site`ライブラリと`clientlib-dependecies`ライブラリを生成します。

[Sass](https://sass-lang.com/)や[TypeScript](https://www.typescriptlang.org/)のような言語をサポートする場合、クライアント側ライブラリにはいくつかの制限があります。 [NPM](https://www.npmjs.com/)や[webpack](https://webpack.js.org/)のようなオープンソースツールが多数あり、フロントエンド開発を高速化し最適化しています。 **ui.frontend**&#x200B;モジュールの目標は、これらのツールを使用して、ほとんどのフロントエンドソースファイルを管理できることです。

1. **ui.frontend**&#x200B;モジュールを開き、`src/main/webpack/site`に移動します。
1. ファイル`main.scss`を開きます

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` は、 `ui.frontend` モジュール内のすべてのSassファイルへのエントリポイントです。`_variables.scss`ファイルが含まれます。このファイルには、プロジェクト内の様々なSassファイル全体で使用する一連のブランド変数が含まれます。 `_base.scss`ファイルも含まれ、HTML要素の基本的なスタイルを定義します。 正規式には、`src/main/webpack/components`以下の個々のコンポーネントスタイルのすべてのスタイルが含まれます。 別の正規式には、`src/main/webpack/site/styles`以下のすべてのファイルが含まれます。

1. `main.ts` ファイルを検査します。`main.ts` プロジェクト `main.scss` 内の任意の `.js` または `.ts` ファイルを収集するための正規式が含まれ、含まれます。このエントリポイントは、[webpack構成ファイル](https://webpack.js.org/configuration/)によって、`ui.frontend`モジュール全体のエントリポイントとして使用されます。

1. `src/main/webpack/site/styles`の下のファイルをInspect:

   ![スタイルファイル](assets/client-side-libraries/style-files.png)

   これらのファイルスタイルは、テンプレート内のグローバル要素(ヘッダー、フッター、メインコンテンツコンテナなど)に対応しています。 これらのファイルのターゲットー内のCSSルールは、異なるHTML要素`header`、`main`、`footer`です。 これらのHTML要素は、前の章[ページとテンプレート](./pages-templates.md)のポリシーで定義されています。

1. `src/main/webpack`の下の`components`フォルダーを展開し、ファイルを調べます。

   ![コンポーネントサスファイル](assets/client-side-libraries/component-sass-files.png)

   各ファイルは、[アコーディオンコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)のようなコアコンポーネントにマッピングされます。 各コアコンポーネントは、[ブロック要素修飾子](https://getbem.com/)またはBEM表記を使用して構築され、スタイル規則を使用して特定のCSSクラスを簡単にターゲットできます。 `/components`の下のファイルは、AEMプロジェクトアーキタイプでコンポーネントごとに異なるBEM規則でスタブアウトされました。

1. WKND基本スタイル&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)**&#x200B;と&#x200B;**unzip**&#x200B;をファイルからダウンロードします。

   ![WKND基本スタイル](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   チュートリアルを高速化するために、コアコンポーネントと記事ページテンプレートの構造に基づくWKNDブランドを実装するSassファイルをいくつか用意しました。

1. `ui.frontend/src`の内容を前の手順のファイルで上書きします。 zipの内容によって次のフォルダーが上書きされます。

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![変更されたファイル](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspectは、WKNDスタイルの実装の詳細を確認するためにファイルを変更しました。

## Inspectui.frontendの統合{#ui-frontend-integration}

**ui.frontend**&#x200B;モジュール[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)に組み込まれた主な統合要素は、webpack/npmプロジェクトからコンパイルされたCSSとJSアーティファクトを取り込み、AEMクライアント側ライブラリに変換します。

![ui.frontendアーキテクチャ統合](assets/client-side-libraries/ui-frontend-architecture.png)

AEMプロジェクトのアーキタイプによって、この統合が自動的に設定されます。 次に、その仕組みを見てみましょう。


1. コマンドラインターミナルを開き、`npm install`コマンドを使用して&#x200B;**ui.frontend**&#x200B;モジュールをインストールします。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 新しいクローンまたはプロジェクトの生成の後、1回だけ実行する必要があります。

1. 同じターミナルで、**ui.frontend**&#x200B;モジュールを構築し、展開します。その際には、`npm run dev`コマンドを使用します。

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > に「ERROR in./src/main/webpack/site/main.scss」に置き換えます。
   > これは通常、`npm install`を実行してから環境が変わったためです。
   > `npm rebuild node-sass`を実行して問題を修正します。 これは、ローカル開発マシンにインストールされている`npm`のバージョンが、`aem-guides-wknd/pom.xml`ファイル内のMaven `frontend-maven-plugin`で使用されているバージョンと異なる場合に発生します。 この問題は、pomファイル内のバージョンをローカルバージョンに合わせて変更するか、ローカルバージョンと同じバージョンに変更することで、永続的に修正できます。

1. `npm run dev`コマンドは、Webpackプロジェクトのソースコードを構築してコンパイルし、最終的には&#x200B;**ui.apps**&#x200B;モジュールの&#x200B;**clientlib-site**&#x200B;と&#x200B;**clientlib-dependencies**&#x200B;に設定する必要があります。

   >[!NOTE]
   >
   >JSとCSSを縮小する`npm run prod`プロファイルもあります。 Mavenを介してWebPackビルドがトリガーされるたびに、標準コンパイルが実行されます。 [ui.frontendモジュールの詳細は、](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)を参照してください。

1. `ui.frontend/dist/clientlib-site/css/site.css`の下の`site.css`ファイルをInspectします。 これは、Sassソースファイルに基づいてコンパイルされたCSSです。

   ![分散サイトCSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` ファイルを検査します。これは、`/dist`の内容をクライアントライブラリに変換し、`ui.apps`モジュールに移動するnpmプラグイン[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)の設定ファイルです。

1. `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`の&#x200B;**ui.apps**&#x200B;モジュールの`site.css`ファイルをInspectします。 これは、**ui.frontend**&#x200B;モジュールの`site.css`ファイルと同じコピーである必要があります。 これで、**ui.apps**&#x200B;モジュールに入ったので、AEMにデプロイできます。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site**&#x200B;はビルド時にコンパイルされるので、**npm**&#x200B;または&#x200B;**maven**&#x200B;を使用します。したがって、**ui.apps**&#x200B;モジュールのソース管理から無視しても問題ありません。 **ui.apps**&#x200B;の下の`.gitignore`ファイルをInspectに置きます。

1. 開発者ツールやMavenスキルを使用して、`clientlib-site`ライブラリをAEMのローカルインスタンスと同期します。

   ![Clientlibサイトの同期](assets/client-side-libraries/sync-clientlib-site.png)

1. AEMのLA Skatepark記事を開きます。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![記事のベーススタイルの更新](assets/client-side-libraries/updated-base-styles.png)

   これで、記事の更新されたスタイルが表示されます。 ブラウザーでキャッシュされたCSSファイルをクリアするには、ハードリフレッシュが必要になる場合があります。

   モックアップの方がずっと近くに見える！

   >[!NOTE]
   >
   > 上記の手順を実行してui.frontendコードをAEMに構築およびデプロイすると、Mavenビルドがプロジェクト`mvn clean install -PautoInstallSinglePackage`のルートからトリガーされたときに、自動的に実行されます。

>[!CAUTION]
>
> **ui.frontend**&#x200B;モジュールの使用は、すべてのプロジェクトで必要とは限りません。 **ui.frontend**&#x200B;モジュールは複雑さを増します。高度なフロントエンドツール（Sass、webpack、npmなど）を使用する必要がない場合は、必要ない場合もあります。

## ページとテンプレートを含める{#page-inclusion}

次に、clientlibsがAEMページでどのように参照されているかを確認します。 Web開発の一般的なベストプラクティスは、`</body>`タグを閉じる直前に、CSSをHTMLヘッダー`<head>`とJavaScriptに含めることです。

1. **ui.apps**&#x200B;モジュールで`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`に移動します。

   ![構造ページコンポーネント](assets/client-side-libraries/customheaderlibs-html.png)

   これは、WKND実装のすべてのページをレンダリングするために使用される`page`コンポーネントです。

1. `customheaderlibs.html` ファイルを開きます。`${clientlib.css @ categories='wknd.base'}`行に注目してください。 これは、カテゴリ`wknd.base`のclientlibのCSSが、このファイルを介して含まれることを示しています。これは、当社の全ページのヘッダーに&#x200B;**clientlib-base**&#x200B;を含めることになります。

1. `customheaderlibs.html`を更新し、**ui.frontend**&#x200B;モジュールで前に指定したGoogleフォントスタイルへの参照を含めます。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. `customfooterlibs.html` ファイルを検査します。`customheaderlibs.html`のように、このファイルはプロジェクトを実装する際に上書きすることを目的としています。 ここで`${clientlib.js @ categories='wknd.base'}`行は、**clientlib-base**&#x200B;のJavaScriptが、すべてのページの末尾に含まれることを意味します。

1. 開発者ツールを使用するか、Mavenのスキルを使用して、`page`コンポーネントをAEMサーバーにエクスポートします。

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)にある記事ページテンプレートを参照します。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**ページポリシー**」を選択して&#x200B;**ページポリシー**&#x200B;ダイアログを開きます。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy.png)

   *ページ情報/ページポリシー*

1. `wknd.dependencies`と`wknd.site`のカテゴリがここに表示されています。 デフォルトでは、ページポリシーで設定したclientlibは分割され、ページのheadにCSSが含まれ、本文の末尾にJavaScriptが含まれます。 必要に応じて、ページのheadにclientlib JavaScriptが読み込まれることを明示的にリストできます。 これは`wknd.dependencies`の場合です。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > また、`wknd.base` clientlibの前述の例のように、`customheaderlibs.html`または`customfooterlibs.html`スクリプトを使用して、ページコンポーネントから直接`wknd.site`または`wknd.dependencies`を参照することもできます。 テンプレートを使用すると、テンプレートごとに使用するclientlibを柔軟に選択できます。 例えば、非常に重いJavaScriptライブラリがあり、選択したテンプレートでのみ使用される場合、

1. **記事ページテンプレート**&#x200B;を使用して作成した&#x200B;**LA Skateparks**&#x200B;ページに移動します。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). フォントの違いが見られるはずです。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**発行済みの表示**」を選択して、AEMエディターの外部で記事ページを開きます。

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

   clientlibsはプロキシ`/etc.clientlibs`エンドポイントを使用しています。 また、次のclientlibインクルードがページの最下部に表示されます。

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4で後に続く場合、クライアント側のライブラリは自動的に縮小されません。 [HTML Library Managerのドキュメントを参照して、最小化（推奨）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)を有効にしてください。

   >[!WARNING]
   >
   >セキュリティ上の理由から、/apps パスは&#x200B;**** Dispatcher の filter セクション&#x200B;**にのみ使用すべきであるので、パブリッシュ側では、クライアントライブラリを /apps から提供**&#x200B;しない[](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)ことが重要となります。クライアントライブラリの[allowProxyプロパティ](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)は、CSSとJSが&#x200B;**/etc.clientlibs**&#x200B;から提供されることを保証します。

## Webpack DevServer — 静的マークアップ{#webpack-dev-static}

前の2つの演習では、**ui.frontend**&#x200B;モジュール内の複数のSassファイルを更新し、構築プロセスを通じて、最終的にこれらの変更がAEMに反映されていることを確認しました。 次に、[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)を活用して、**静的** HTMLに対するフロントエンドスタイルを迅速に開発するテクニックを見てみましょう。

この手法は、AEM環境に容易にアクセスできない可能性のある専用のフロントエンド開発者が、スタイルとフロントエンドコードの大部分を実行する場合に便利です。 また、この手法により、FEDがHTMLに直接変更を加え、それをAEM開発者に渡して、コンポーネントとして実装することもできます。

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)にあるLA skatepark記事ページのページソースをコピーします。
1. IDEを再度開きます。 コピーしたマークアップをAEMから`src/main/webpack/static`の下の&#x200B;**ui.frontend**&#x200B;モジュールの`index.html`に貼り付けます。
1. コピーしたマークアップを編集し、**clientlib-site**&#x200B;と&#x200B;**clientlib-dependencies**&#x200B;への参照を削除します。

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   これらの参照は削除できます。webpack開発サーバーによってこれらのアーティファクトが自動的に生成されるからです。

1. **ui.frontend**&#x200B;モジュール内で次のコマンドを実行し、新しい端末からwebpack開発サーバを開始します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. これにより、[http://localhost:8080/](http://localhost:8080/)に静的なマークアップで新しいブラウザウィンドウが開きます。

1. ファイル`src/main/webpack/site/_variables.scss`を編集します。 `$text-color`ルールを次のルールに置き換えます。

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   変更内容を保存します。

1. 変更は自動的に[http://localhost:8080](http://localhost:8080)のブラウザーに反映されます。

   ![ローカルWebPack開発サーバーの変更](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js`ファイルを確認します。 webpack-dev-serverの開始に使用するwebpack設定が含まれます。 AEMのローカルで実行されているインスタンスから`/content`と`/etc.clientlibs`のパスをプロキシします。 これは、イメージや他のclientlib （**ui.frontend**&#x200B;コードでは管理されていない）を利用できるようにする方法です。

   >[!CAUTION]
   >
   > 静的マークアップの画像srcは、ローカルAEMインスタンス上のライブ画像コンポーネントを指します。 画像へのパスが変更された場合、AEMが起動されていない場合、またはブラウザーがローカルのAEMインスタンスにログインしていない場合、画像は壊れて表示されます。 外部リソースに渡す場合は、画像を静的参照に置き換えることもできます。

1. **コマンドラインから`CTRL+C`と入力して、WebPackサーバーを停止**&#x200B;できます。

## Webpack DevServer — 監視とaemsync {#webpack-dev-watch}

もう1つの方法は、Node.jsに`ui.frontend`モジュール内のsrcファイルに対するファイル変更を監視させることです。 ファイルが変更されると、クライアントライブラリがすばやくコンパイルされ、[aemsync](https://www.npmjs.com/package/aemsync) npmモジュールを使用して、実行中のAEMサーバに変更を同期します。

1. **ui.frontend**&#x200B;モジュール内から次のコマンドを実行して、新しい端末からwebpack devサーバーを&#x200B;**watch**&#x200B;モードで開始します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. これは`src`ファイルをコンパイルし、[http://localhost:4502](http://localhost:4502)でAEMと変更を同期します。

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

1. AEMに移動し、LA Skateparksの記事を表示します。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![AEMに展開された変更](assets/client-side-libraries/changes-deployed-aem-watch.png)

   変更をAEMにデプロイする必要があります。 若干遅延が生じるので、更新を表示するには、手動でブラウザーを更新する必要があります。 ただし、新しいコンポーネントやダイアログのオーサリングを使用する場合は、AEMで変更を直接表示すると便利です。

1. 変更を`_variables.scss`に戻し、変更を保存します。 わずかな遅延が発生した後、変更はAEMのローカルインスタンスと再び同期されます。

1. WebPack Devサーバーを停止し、プロジェクトのルートから完全なMavenビルドを実行します。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   再び`ui.frontend`モジュールがコンパイルされ、クライアントライブラリに変換され、`ui.apps`モジュールを介してAEMにデプロイされます。 しかし今回はMavenが私たちのために全てを行います

## これで完了です! {#congratulations}

おめでとうございます。記事ページにはWKNDブランドと一致する一貫したスタイルがいくつか用意され、**ui.frontend**&#x200B;モジュールに慣れています。

### 次の手順 {#next-steps}

Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法を説明します。 [スタイル](style-system.md) システムを使用した開発では、スタイルシステムを使用して、コアコンポーネントをブランド固有のCSSおよびテンプレートエディターの高度なポリシー設定で拡張する方法について説明します。

[GitHub](https://github.com/adobe/aem-guides-wknd)上の完了したコードを表示するか、Gitブラック`tutorial/client-side-libraries-solution`上のローカルにコードを確認して展開します。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。
1. `tutorial/client-side-libraries-solution`ブランチを調べます。

## その他のツールとリソース{#additional-resources}

### aemfed{#develop-aemfed} 

[**aemfedは、フロントエンドの開発速度を上げるために使用できるオープンソースのコマンドラインツールです。**](https://aemfed.io/) [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync), [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)で動作します。

**aemfed**&#x200B;は、**ui.apps**&#x200B;モジュール内のファイル変更をリッスンし、実行中のAEMインスタンスに自動的に同期するように設計されています。 変更に基づき、ローカルブラウザーが自動更新されるので、フロントエンド開発がスピードアップします。また、Slingログトレーサと連携して、サーバ側のエラーを端末に直接表示するように設計されています。

**ui.apps**&#x200B;モジュール内で多くの作業を行っている場合、HTLスクリプトを変更し、カスタムコンポーネントを作成すると、**aemfed**&#x200B;は非常に強力なツールとなります。 [完全なドキュメントは、こちらを参照してください。](https://github.com/abmaonline/aemfed).

### クライアント側ライブラリのデバッグ{#debugging-clientlibs}

**カテゴリ**&#x200B;と&#x200B;**は異なる方法で**&#x200B;を埋め込み、複数のクライアントライブラリを含めるので、トラブルシューティングが面倒になります。 AEM はそのためにいくつかのツールを公開しています。最も重要なツールの1つは、**Rebuild Client Libraries**&#x200B;です。これは、AEMにLESSファイルを再コンパイルさせ、CSSを生成させるよう強制します。

* [**Libsのダンプ**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -AEMインスタンスに登録されているすべてのクライアントライブラリをリストします。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  -カテゴリに基づいてclientlibインクルードのHTML出力が期待通りに表示されることをユーザーに示します。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Libraries Dependencies validation**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 見つからない依存関係や埋め込みカテゴリが強調表示されます。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**クライアントライブラリの再ビルド**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM はすべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページの更新をおこなう方が、すべてのライブラリを再ビルドするよりも効果的です。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![クライアントライブラリを再構築](assets/client-side-libraries/rebuild-clientlibs.png)
