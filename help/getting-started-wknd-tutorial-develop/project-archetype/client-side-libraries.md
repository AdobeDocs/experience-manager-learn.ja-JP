---
title: クライアントライブラリとフロントエンドワークフロー
description: クライアントライブラリを使用して、Adobe Experience Manager（AEM）Sites 実装の CSS と JavaScript をデプロイおよび管理する方法について説明します。 Webpack プロジェクトである ui.frontend モジュールをエンドツーエンドのビルドプロセスに統合する方法について説明します。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2554'
ht-degree: 100%

---

# クライアントライブラリとフロントエンドワークフロー {#client-side-libraries}

このチュートリアルでは、クライアントサイドライブラリ（clientlib）を使用して、Adobe Experience Manager （AEM） Sites 実装の CSS と JavaScript をデプロイおよび管理する方法について説明します。このチュートリアルでは、[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja) モジュール、非結合 [webpack](https://webpack.js.org/) プロジェクトを、エンドツーエンドのビルドプロセスに統合する方法についても説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)を設定するために必要なツールや説明を確認します。

[コンポーネントの基本](component-basics.md#client-side-libraries)チュートリアルを確認して、クライアントサイドライブラリと AEM の基礎を理解することもお勧めします。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの作成元となるベースラインコードをチェックアウトします。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の `tutorial/client-side-libraries-start` ブランチをチェックアウトします。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Maven スキルを使用して、ローカル AEM インスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 または 6.4 を使用している場合は、任意の Maven コマンドに `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

いつでも、完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) で確認したり、ブランチ `tutorial/client-side-libraries-solution` に切り替えてコードをローカルにチェックアウトしたりできます。

## 目的

1. 編集可能なテンプレートを使用してクライアントサイドライブラリをページに組み込む方法を理解する。
1. 専用のフロントエンド開発用の webpack 開発サーバーと `ui.frontend` モジュールの使用方法を確認する。
1. コンパイル済みの CSS と JavaScript を Sites 実装に配信するエンドツーエンドのワークフローについて理解する

## 作ろうとしているもの {#what-build}

この章では、WKND サイトと記事ページテンプレートのベースラインスタイルを追加して、実装を [UI デザインモックアップ](assets/pages-templates/wknd-article-design.xd)に近づけます。高度なフロントエンドワークフローを使用して、webpack プロジェクトを AEM クライアントライブラリに統合します。

![完成したスタイル](assets/client-side-libraries/finished-styles.png)

*ベースラインスタイルが適用された記事ページ*

## 背景 {#background}

クライアントサイドライブラリは、AEM Sites の実装に必要な CSS と JavaScript ファイルの編成および管理のための仕組みを提供します。クライアントライブラリ（clientlib）の基本的な目的は、以下のとおりです。

1. CSS／JS を、開発および管理が簡単な個別の小さなファイルに保存する
1. 組織的な方法で、サードパーティのフレームワークへの依存関係を管理する
1. CSS／JS を 1～2 個の要求に連結することで、クライアント側の要求数を最小限にする.

クライアントサイドライブラリの使用の詳細については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja)を参照してください。

クライアントサイドライブラリにはいくつかの制限があります。 中でも注目すべきは、Sass、LESS、TypeScript などの一般的なフロントエンド言語のサポートが制限されていることです。 このチュートリアルでは、**ui.frontend** モジュールがこの問題の解決にどう役立つのかを見ていきます。

スターターコードベースをローカルの AEMインスタンスにデプロイし、[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) に移動します。このページのスタイルは設定されていません。 WKND ブランドのクライアントサイドライブラリを実装して、ページに CSS と JavaScript を追加しましょう。

## クライアントサイドライブラリの組織 {#organization}

次に、 [AEM プロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)で生成された clientlib の組織を見ていきます。

![クライアントライブラリ組織の概要](./assets/client-side-libraries/high-level-clientlib-organization.png)

*クライアントサイドライブラリの組織とページを含む概要図*

>[!NOTE]
>
> 次のクライアントサイドライブラリ組織は、AEM プロジェクトアーキタイプによって生成されますが、これは出発点にすぎません。 プロジェクトが CSS と JavaScript を最終的に管理して Sites 実装に提供する方法は、リソース、スキルセット、要件に応じて、大きく異なる場合があります。

1. VSCode または他の IDE を使用して、**ui.apps** モジュールを開きます。
1. パス `/apps/wknd/clientlibs` を展開して、アーキタイプで生成された clientlib を表示します。

   ![ui.apps の clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   以下のセクションでは、これらの clientlib の詳細について説明します。

1. 次の表に、クライアントライブラリの概要を示します。クライアントライブラリを含める方法については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=ja#developing?)を参照してください。

   | 名前 | 説明 | 備考 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND サイトが機能するために必要となる CSS および JavaScript の基本レベルを構成します | コアコンポーネントのクライアントライブラリを埋め込みます |
   | `clientlib-grid` | [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=ja)が機能するのに必要な CSS を生成します  | モバイルとタブレットのブレークポイントは、ここで設定できます |
   | `clientlib-site` | WKND サイト固有のテーマを含みます | `ui.frontend` モジュールによって生成されます |
   | `clientlib-dependencies` | サードパーティの依存関係を埋め込みます | `ui.frontend` モジュールによって生成されます |

1. その `clientlib-site` を確認します。`clientlib-dependencies` はソース管理から無視されます。 これは意図的なもので、ビルド時に `ui.frontend` モジュールによって生成されます。

## 基本スタイルの更新 {#base-styles}

次に、 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja)** モジュールで定義された基本スタイルを更新します。 `ui.frontend` モジュールのファイル は、サイトテーマとサードパーティの依存関係を含む `clientlib-site` および `clientlib-dependecies` のライブラリを生成します。

クライアントサイドライブラリは、[Sass](https://sass-lang.com/) または [TypeScript](https://www.typescriptlang.org/) などのより高度な言語をサポートしていません。フロントエンド開発を加速させて最適化する [NPM](https://www.npmjs.com/) や [webpack](https://webpack.js.org/) などのオープンソースツールがいくつかあります。 **ui.frontend** モジュールの目標は、これらのツールを使用して、ほとんどのフロントエンドソースファイルを管理できるようにすることです。

1. **ui.frontend** モジュールを開き、`src/main/webpack/site` に移動します。
1. ファイル `main.scss` を開きます

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` は、 `ui.frontend` モジュールの Sass ファイルへのエントリポイントです。 これには、プロジェクト内の様々な Sass ファイル全体で使用される一連のブランド変数を含む `_variables.scss` ファイルが含まれます。 また、`_base.scss` ファイルも含まれ、HTML 要素の基本スタイルを定義します。 正規表現には、`src/main/webpack/components` にあるの個々のコンポーネントのスタイルが含まれます。別の正規表現には、`src/main/webpack/site/styles` にあるファイルが含まれます。

1. `main.ts` ファイルを調べます。これには `main.scss` のほか、プロジェクトで `.js` または `.ts` ファイルを収集するための正規表現が含まれます。 このエントリポイントは、`ui.frontend` モジュール全体で [webpack 設定ファイル](https://webpack.js.org/configuration/)によって使用されます。

1. `src/main/webpack/site/styles` の下にあるファイルを調べます。

   ![スタイルファイル](assets/client-side-libraries/style-files.png)

   これらのファイルは、ヘッダー、フッター、メインコンテンツコンテナなど、テンプレート内のグローバル要素のスタイルを設定します。 これらのファイルの CSS ルールは、様々な HTML 要素（`header`、`main`、および  `footer`）をターゲットにします。 これらの HTML 要素は、前の章の[ページとテンプレート](./pages-templates.md)のポリシーによって定義されています。

1. `src/main/webpack` の下の `components` フォルダーを展開してファイルを調べます。

   ![コンポーネントの Sass ファイル](assets/client-side-libraries/component-sass-files.png)

   各ファイルは、[アコーディオンコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=ja)といったコアコンポーネントにマッピングされます。スタイルルールを使用して特定の CSS クラスを容易にターゲット設定できるよう、各コアコンポーネントは[ブロック要素修飾子](https://getbem.com/)または BEM 表記で作成されます。 `/components` の下にあるファイルは、コンポーネントごとに異なる BEM ルールを使用して、AEM プロジェクトアーキタイプによってスタブ化されています。

1. WKND の基本スタイル **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** をダウンロードし、ファイルを&#x200B;**解凍**&#x200B;します。

   ![WKND の基本スタイル](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   チュートリアルの進行を促進するために、コアコンポーネントと記事ページテンプレートの構造に基づく WKND ブランドを実装する Sass ファイルがいくつか用意されています。

1. `ui.frontend/src` の内容を前の手順からのファイルで上書きします。ZIP された内容で、次のフォルダーを上書きします。

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![変更されたファイル](assets/client-side-libraries/changed-files-uifrontend.png)

   WKND スタイルの実装の詳細を確認するために、変更されたファイルを検査します。

## ui.frontend 統合の検査 {#ui-frontend-integration}

**ui.frontend** モジュールに組み込まれた主要な統合要素である [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) は、コンパイル済みの CSS および JS アーティファクトを webpack/npm プロジェクトから取得し、AEM クライアントサイドライブラリに変換します。

![ui.frontend アーキテクチャ統合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM プロジェクトアーキタイプは、この統合を自動的に設定します。次に、その仕組みを見てみましょう。


1. コマンドラインターミナルを開き、**ui.frontend** モジュールを `npm install` コマンドを使用してインストールします。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >新しいクローンやプロジェクトの生成の後と同様に、`npm install` の実行は 1 回だけ必要です。

1. `ui.frontend/package.json` を開き、**スクリプト****開始**&#x200B;コマンドに `--env writeToDisk=true` を追加します。

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. **監視**&#x200B;モードで webpack 開発サーバーを起動するには、次のコマンドを実行します。

   ```shell
   $ npm run watch
   ```

1. これは `ui.frontend` モジュールからソースファイルをコンパイルし、[http://localhost:4502](http://localhost:4502) で AEM と変更を同期します。

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

1. コマンド `npm run watch` で最終的に **clientlib-site** および **clientlib-dependencies** を AEM と自動的に同期される **ui.apps** モジュールで入力します。

   >[!NOTE]
   >
   >また、JS と CSS を縮小する `npm run prod` プロファイルがあります。Maven 経由で webpack ビルドがトリガーされるたびに行われる標準的なコンパイルです。[ui.frontend モジュールについて詳しくは、こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja)を参照してください。

1. ファイル `site.css` を `ui.frontend/dist/clientlib-site/site.css` 下で検査します。これは、Sass ソースファイルに基づくコンパイル済み CSS です。

   ![分散サイト CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` ファイルを調べます。これは [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) という npm プラグインの設定ファイルで、`/dist` の内容をクライアントライブラリに変換し、`ui.apps` モジュールに移動させます。

1. `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css` でファイル `site.css` を **ui.apps** モジュールで検査します。これは、**ui.frontend** モジュールからの `site.css` ファイルと同一コピーである必要があります。**ui.apps** モジュールで AEM にデプロイできるようになりました。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site** は、**npm** または **maven** を使用してビルド時にコンパイルされているため、**ui.apps** モジュールでソース管理から安全に無視できます。**ui.apps** 下の `.gitignore` ファイルを検査します。

1. AEM の LA スケートパークに関する記事を [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) で開きます。

   ![記事のベーススタイルを更新しました](assets/client-side-libraries/updated-base-styles.png)

   これで、記事の更新されたスタイルが表示されます。ブラウザーでキャッシュされた CSS ファイルをクリアするには、ハード更新が必要な場合があります。

   外観がモックアップにかなり近づいてきました。

   >[!NOTE]
   >
   > ui.frontend コードをビルドして AEM にデプロイするための上記の手順は、Maven ビルドが `mvn clean install -PautoInstallSinglePackage` プロジェクトのルートからトリガーされたときに自動的に実行されます。

## スタイルの変更

次に、`ui.frontend` モジュールで小規模な変更を加えて、`npm run watch` がローカルの AEM インスタンスにスタイルを自動的にデプロイする様子を確認します。

1. `ui.frontend` モジュールからファイル `ui.frontend/src/main/webpack/site/_variables.scss` を開きます。
1. `$brand-primary` カラー変数を更新します。

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   変更を保存します。

1. ブラウザーに戻り、AEM ページを更新して更新内容を確認します。

   ![クライアントサイドライブラリ](assets/client-side-libraries/style-update-brand-primary.png)

1. `$brand-primary` カラーの変更を元に戻し、コマンド `CTRL+C` を使用して webpack ビルドを停止します。

>[!CAUTION]
>
> **ui.frontend** モジュールの使用は、必ずしもすべてのプロジェクトに必要とは限りません。**ui.frontend** モジュールを使用すると、より複雑になるので、これらの高度なフロントエンドツール（Sass、webpack、npm など）のいくつかを使用する必要や希望がなければ、使用しなくてもよい可能性があります。

## ページとテンプレートの組み込み {#page-inclusion}

次に、clientlib が AEM ページでどのように参照されているかを確認します。Web 開発の一般的なベストプラクティスは、CSS を HTML ヘッダー `<head>` に含め、JavaScript を終了タグ `</body>` の直前で組み込むことです。

1. Article Page テンプレート（[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)）を参照してください。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**ページポリシー**」を選択して&#x200B;**ページポリシー**&#x200B;ダイアログを開きます。

   ![Article Page テンプレートメニューのページポリシー](assets/client-side-libraries/template-page-policy.png)

   *ページ情報／ページポリシー*

1. ここには `wknd.dependencies` と `wknd.site` のカテゴリが一覧表示されていることに注目してください。デフォルトでは、ページポリシーを通じて設定される clientlib は分割されて、CSS がページヘッダーに含まれ、JavaScript が本体の末尾に含まれます。ページヘッダーで読み込まれる clientlib JavaScript を明示的にリストすることができます。以下は `wknd.dependencies` の場合です。

   ![Article Page テンプレートメニューのページポリシー](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > `customheaderlibs.html` または `customfooterlibs.html` スクリプトを使用して、ページコンポーネントから直接 `wknd.site` または `wknd.dependencies` を参照することもできます。テンプレートを使用すると、テンプレートごとに使用する clientlib を柔軟に選択できます。例えば、選択したテンプレートでのみ使用される大きな JavaScript ライブラリがある場合です。

1. **Article Page テンプレート**&#x200B;を使用して作成した **LA Skateparks** ページ（[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)）に移動します。

1. **ページ情報**&#x200B;アイコンをクリックし、メニューで「**公開済みとして表示**」を選択して AEM エディターの外部で記事ページを開きます。

   ![公開済みとして表示](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) のページソースを表示すると、`<head>` に次の clientlib 参照が含まれていることがわかります。

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   なお、これらの clientlibs はプロキシ `/etc.clientlibs` エンドポイントを使用しています。また、次の clientlib がページの下部に含まれていることもわかります。

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > AEM 6.5／6.4 の場合、クライアントサイドライブラリは自動的には縮小されません。[HTML ライブラリマネージャーによる縮小の有効化（推奨）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja#using-preprocessors)に関するドキュメントを参照してください。

   >[!WARNING]
   >
   >セキュリティ上の理由から、/apps パスは&#x200B;**** Dispatcher の filter セクション&#x200B;**にのみ使用すべきなので、パブリッシュ側では、クライアントライブラリを /apps から提供**&#x200B;しない[](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#example-filter-section)ことが重要となります。クライアントライブラリの [allowProxy プロパティ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)により、CSS と JavaScript が **/etc.clientlibs** から提供されるようになります。

### 次の手順 {#next-steps}

個々のスタイルを実装し、Experience Manager のスタイルシステムを使用してコアコンポーネントを再利用する方法について説明します。 [スタイルシステムを使った開発](style-system.md)では、スタイルシステムを使用してブランド固有の CSS とテンプレートエディターの高度なポリシー設定でコアコンポーネントを拡張する方法について説明します。

完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd) で確認したり、Git ブランチ `tutorial/client-side-libraries-solution` でレビューしてローカルにデプロイしたりできます。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) リポジトリのクローンを作成します。
1. `tutorial/client-side-libraries-solution` ブランチをチェックアウトします。

## その他のツールとリソース {#additional-resources}

### Webpack DevServer - 静的マークアップ {#webpack-dev-static}

前の演習では、**ui.frontend** モジュールのいくつかの Sass ファイルが更新され、ビルドプロセスを通じて最終的にこれらの変更が AEM に反映されることを確認しました。次は、[webpack-dev-server](https://webpack.js.org/configuration/dev-server/) を使用して&#x200B;**静的** HTML に対するフロントエンドスタイルを迅速に開発する手法を見ていきます。

この手法は、AEM 環境に容易にアクセスできない専任のフロントエンド開発者（FED）によって、ほとんどのスタイルとフロントエンドコードが実行される場合に便利です。また、この手法を使用すると、FED が HTML に直接変更を加え、それを AEM 開発者に渡してコンポーネントとして実装してもらうこともできます。

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) で LA スケートパークの記事ページのページソースをコピーします。
1. IDE を再度開きます。 AEM からコピーしたマークアップを `src/main/webpack/static` の下にある **ui.frontend** モジュールの `index.html` に貼り付けます。
1. コピーしたマークアップを編集して、**clientlib-site** および **clientlib-dependencies** への参照をすべて削除します。

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   webpack 開発サーバーがこれらのアーティファクトを自動的に生成するので、これらの参照を削除します。

1. **ui.frontend** モジュール内から次のコマンドを実行して、新しいターミナルから webpack サーバーを起動します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. これにより、[http://localhost:8080/](http://localhost:8080/) で新しいブラウザーウィンドウが開き、静的マークアップが表示されます。

1. `src/main/webpack/site/_variables.scss` ファイルを編集します。`$text-color` ルールを次のように置き換えます。

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   変更を保存します。

1. ブラウザーで [http://localhost:8080](http://localhost:8080) に自動的に反映された変更が表示されるはずです。

   ![ローカル webpack 開発サーバーの変更](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js` ファイルを確認します。webpack-dev-server の起動に使用する Webpack 設定が含まれます。 ローカルで実行されている AEM のインスタンスからパス `/content` と `/etc.clientlibs` をプロキシします。これにより、画像や他の clientlib（**ui.frontend** コードによって管理されていない）が使用可能になります。

   >[!CAUTION]
   >
   > 静的マークアップの画像ソースは、ローカル AEM インスタンス上のライブ画像コンポーネントを指しています。 画像のパスが変更された場合、AEM が起動されていない場合、ブラウザーがローカルの AEM インスタンスにログインしていない場合は、画像が壊れて表示されます。 外部リソースに渡す場合は、画像を静的な参照で置き換えることもできます。

1. コマンドラインに `CTRL+C` と入力すると、webpack サーバーを&#x200B;**停止**&#x200B;できます。

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** は、フロントエンド開発の高速化に使用できるオープンソースのコマンドラインツールです。 [aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://browsersync.io/)、[Sling ログトレーサー](https://sling.apache.org/documentation/bundles/log-tracers.html)を備えています。

大まかに言うと、`aemfed` は **ui.apps** モジュール内のファイルの変更をリッスンし、それらを実行中の AEM インスタンスに直接自動的に同期するように設計されています。変更に基づき、ローカルブラウザーが自動更新されるので、フロントエンド開発がスピードアップします。aemfed は Sling ログトレーサーとも連携し、サーバーサイドのすべてのエラーを直接ターミナルに自動表示します。

**ui.apps** モジュール内で多くの作業を行い、HTL スクリプトを修正し、カスタムコンポーネントを作成する場合、**aemfed** は強力なツールとして使用できます。[完全なドキュメントについては、こちら](https://github.com/abmaonline/aemfed)を参照してください。

### クライアントサイドライブラリのデバッグ {#debugging-clientlibs}

複数のクライアントライブラリを組み込むために、**カテゴリ**&#x200B;および&#x200B;**埋め込み**&#x200B;の様々な方法を使用すると、トラブルシューティングが面倒になることがあります。AEM はそのためにいくつかのツールを公開しています。最も重要なツールの 1 つは、**クライアントライブラリの再ビルド**&#x200B;です。これにより、AEM はすべての LESS ファイルを強制的に再コンパイルし、CSS を生成します。

* [**ライブラリのダンプ**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM インスタンスに登録されているすべてのクライアントライブラリを一覧表示します。`<host>/libs/granite/ui/content/dumplibs.html`

* [**出力テスト**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - カテゴリ別を含む、clientlib の予想される HTML 出力を確認します。`<host>/libs/granite/ui/content/dumplibs.test.html`

* [**ライブラリの依存関係の検証**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 見つからないすべての依存関係または埋め込みカテゴリを強調表示します。`<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**クライアントライブラリの再ビルド**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEM ですべてのクライアントライブラリを強制的に再ビルドするか、クライアントライブラリのキャッシュを無効にできます。このツールでは、AEM が生成された CSS を強制的に再コンパイルするので、LESS を使用した開発において特に効果的です。一般的に、キャッシュを無効化した後にページを更新する方が、すべてのライブラリを再ビルドするよりも効果的です。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![クライアントライブラリを再構築](assets/client-side-libraries/rebuild-clientlibs.png)
