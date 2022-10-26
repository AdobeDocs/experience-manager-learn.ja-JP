---
title: クライアントライブラリとフロントエンドワークフロー
description: クライアントライブラリまたは clientlib を使用して、Adobe Experience Manager(AEM)Sites 実装の CSS と JavaScript をデプロイおよび管理する方法について説明します。 Webpack プロジェクトである ui.frontend モジュールをエンドツーエンドのビルドプロセスに統合する方法について説明します。
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '2825'
ht-degree: 8%

---

# クライアントライブラリとフロントエンドワークフロー {#client-side-libraries}

Adobe Experience Manager(AEM)Sites 実装で CSS と JavaScript をデプロイおよび管理するために、クライアントサイドライブラリ (clientlib) を使用する方法について説明します。 このチュートリアルでは、 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja) モジュール、非結合 [webpack](https://webpack.js.org/) プロジェクトを、エンドツーエンドのビルドプロセスに統合することができます。

## 前提条件 {#prerequisites}

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

また、 [コンポーネントの基本](component-basics.md#client-side-libraries) クライアント側ライブラリとAEMの基本を理解するためのチュートリアルです。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの構築元となるベースラインコードを確認します。

1. 以下を確認します。 `tutorial/client-side-libraries-start` ～から分岐する [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Maven のスキルを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 または 6.4 を使用している場合、 `classic` 任意の Maven コマンドに対するプロファイル。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `tutorial/client-side-libraries-solution`.

## 目的

1. 編集可能なテンプレートを使用してクライアント側ライブラリをページに組み込む方法を理解します。
1. 専用のフロントエンド開発に UI.Frontend Module と Webpack 開発サーバーを使用する方法を説明します。
1. コンパイル済みの CSS と JavaScript を Sites 実装に配信するエンドツーエンドのワークフローを理解します。

## 作成する内容 {#what-you-will-build}

この章では、WKND サイトと記事ページテンプレートのベースラインスタイルを追加して、実装を [UI デザインモックアップ](assets/pages-templates/wknd-article-design.xd). 高度なフロントエンドワークフローを使用して、Webpack プロジェクトをAEMクライアントライブラリに統合します。

![完成したスタイル](assets/client-side-libraries/finished-styles.png)

*ベースラインスタイルが適用された記事ページ*

## 背景 {#background}

クライアントサイドライブラリは、AEM Sitesの実装に必要な CSS および JavaScript ファイルを整理および管理するメカニズムを提供します。 クライアント側ライブラリまたは clientlib の基本的な目標は次のとおりです。

1. CSS／JS を、開発および管理が簡単な個別の小さなファイルに保存する
1. 組織化された方法でサードパーティのフレームワークへの依存関係を管理
1. CSS／JS を 1～2 個の要求に連結することで、クライアント側の要求数を最小限にする.

クライアント側ライブラリの使用の詳細については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja)を参照してください。

クライアント側ライブラリにはいくつかの制限があります。 最も顕著なのは、Sass、LESS、TypeScript などの一般的なフロントエンド言語のサポートが制限されていることです。 このチュートリアルでは、 **ui.frontend** モジュールはこの問題の解決に役立ちます。

スターターコードベースをローカルAEMインスタンスにデプロイし、に移動します。 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). このページのスタイルは設定されていません。 WKND ブランドのクライアント側ライブラリを実装して、ページに CSS と JavaScript を追加しましょう。

## クライアント側ライブラリ組織 {#organization}

次に、 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja).

![高レベルのクライアントライブラリ組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*クライアント側ライブラリの組織とページを含む概要図*

>[!NOTE]
>
> 次のクライアント側ライブラリ組織は、AEMプロジェクトアーキタイプによって生成されますが、出発点にすぎません。 プロジェクトが CSS と JavaScript を最終的に管理して Sites 実装に提供する方法は、リソース、スキルセット、要件に応じて、大きく異なる場合があります。

1. VSCode または他の IDE を使用して、 **ui.apps** モジュール。
1. パスを展開します。 `/apps/wknd/clientlibs` を使用して、アーキタイプで生成された clientlib を表示します。

   ![ui.apps の clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   これらの clientlib を、以下で詳しく調べます。

1. 次の表に、クライアントライブラリの概要を示します。 詳細： [クライアントライブラリを含めるには、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 名前 | 説明 | 備考 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND サイトが機能するために必要な CSS と JavaScript の基本レベル | コアコンポーネントのクライアントライブラリを埋め込みます。 |
   | `clientlib-grid` | に必要な CSS を生成します [レイアウトモード](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) を有効にします。 | モバイル/タブレットのブレークポイントは、ここで設定できます |
   | `clientlib-site` | WKND サイトのサイト固有のテーマを含む | 生成元 `ui.frontend` モジュール |
   | `clientlib-dependencies` | サードパーティの依存関係を埋め込む | 生成元 `ui.frontend` モジュール |

1. ご覧ください `clientlib-site` および `clientlib-dependencies` はソース管理から無視されます。 これは設計に基づいており、ビルド時に `ui.frontend` モジュール。

## 基本スタイルを更新 {#base-styles}

次に、 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** モジュール。 のファイル `ui.frontend` モジュールは `clientlib-site` および `clientlib-dependecies` Site テーマとサードパーティの依存関係を含むライブラリ。

クライアント側ライブラリは、 [サス](https://sass-lang.com/) または [TypeScript](https://www.typescriptlang.org/). 次のようなオープンソースツールがいくつかあります。 [NPM](https://www.npmjs.com/) および [webpack](https://webpack.js.org/) フロントエンド開発を高速化および最適化するためのツールです。 の目標 **ui.frontend** モジュールは、これらのツールを使用して、ほとんどのフロントエンドソースファイルを管理できます。

1. を開きます。 **ui.frontend** モジュールに移動し、 `src/main/webpack/site`.
1. ファイルを開きます。 `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
client-side-libraries/main-scss

   `main.scss` は、 `ui.frontend` モジュール。 これには、 `_variables.scss` ファイル。プロジェクト内の様々な Sass ファイル全体で使用される一連のブランド変数を含みます。 この `_base.scss` ファイルも含まれ、HTML要素の基本スタイルを定義します。 正規表現には、以下の個々のコンポーネントスタイルのスタイルが含まれます。 `src/main/webpack/components`. 別の正規表現には、 `src/main/webpack/site/styles`.

1. `main.ts` ファイルを検査します。これには以下が含まれます。 `main.scss` また、 `.js` または `.ts` ファイルをプロジェクトに含めます。 このエントリポイントは、 [webpack 設定ファイル](https://webpack.js.org/configuration/) を、 `ui.frontend` モジュール。

1. Inspectの下のファイル `src/main/webpack/site/styles`:

   ![スタイルファイル](assets/client-side-libraries/style-files.png)

   これらのファイルは、ヘッダー、フッター、メインコンテンツコンテナなど、テンプレート内のグローバル要素のスタイルを設定します。 これらのファイルの CSS ルールは、様々なHTML要素をターゲットにします `header`, `main`、および  `footer`. これらのHTML要素は、前の章のポリシーによって定義されました。 [ページとテンプレート](./pages-templates.md).

1. を展開します。 `components` フォルダーの下 `src/main/webpack` ファイルを検査します。

   ![Component Sass ファイル](assets/client-side-libraries/component-sass-files.png)

   各ファイルは、 [アコーディオンコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). 各コアコンポーネントは、 [ブロック要素修飾子](https://getbem.com/) または BEM 表記を使用して、スタイルルールを使用して特定の CSS クラスを容易にターゲット設定できるようにします。 の下のファイル `/components` は、コンポーネントごとに異なる BEM ルールを使用してAEMプロジェクトアーキタイプによってスタブ化されています。

1. WKND ベーススタイルのダウンロード **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** および **unzip** ファイル。

   ![WKND ベーススタイル](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   チュートリアルを高速化するために、コアコンポーネントと記事ページテンプレートの構造に基づいて WKND ブランドを実装するいくつかの Sass ファイルを提供しました。

1. 次の内容を上書き `ui.frontend/src` 前の手順のファイルを含む zip の内容によって次のフォルダーが上書きされます。

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![変更されたファイル](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect WKND スタイルの実装の詳細を確認するための変更済みファイル。

## Inspect ui.frontend 統合 {#ui-frontend-integration}

に組み込まれた主要な統合要素 **ui.frontend** モジュール [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) は、コンパイル済みの CSS および JS アーティファクトを webpack/npm プロジェクトから取得し、AEMクライアント側ライブラリに変換します。

![ui.frontend アーキテクチャ統合](assets/client-side-libraries/ui-frontend-architecture.png)

AEMプロジェクトアーキタイプは、この統合を自動的に設定します。 次に、その仕組みを見てみましょう。


1. コマンドラインターミナルを開き、 **ui.frontend** モジュールを `npm install` コマンド：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` プロジェクトの新しいクローンまたは生成後に実行する必要があるのは 1 回だけです。

1. で webpack 開発サーバーを起動します。 **監視** モードにするには、次のコマンドを実行します。

   ```shell
   $ npm run watch
   ```

1. これにより、 `src` ファイルを `ui.frontend` 変更をAEM ( ) にモジュール化して同期します。 [http://localhost:4502](http://localhost:4502)

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

1. コマンド `npm run watch` 最終的にはが入力されます **clientlib-site** および **clientlib-dependencies** 内 **ui.apps** AEMと自動的に同期されるモジュール。

   >[!NOTE]
   >
   >また、 `npm run prod` JS と CSS を縮小するプロファイル。 Maven 経由で Webpack ビルドがトリガーされる場合、これは常に標準コンパイルです。 詳細： [ui.frontend モジュールは、こちらをご覧ください。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspectファイル `site.css` 下 `ui.frontend/dist/clientlib-site/site.css`. これは、Sass ソースファイルに基づくコンパイル済み CSS です。

   ![分散サイト CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` ファイルを検査します。これは、npm プラグインの設定ファイルです。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) ～の内容を変える `/dist` をクライアントライブラリに移動し、 `ui.apps` モジュール。

1. Inspectファイル `site.css` 内 **ui.apps** モジュール `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. これは、 `site.css` ファイルを **ui.frontend** モジュール。 これで、 **ui.apps** モジュールをAEMにデプロイできます。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 次以降 **clientlib-site** は、ビルド時にコンパイルされます。 **npm**&#x200B;または **maven**&#x200B;の場合、 **ui.apps** モジュール。 Inspect `.gitignore` の下のファイル **ui.apps**.

1. AEMの LA Skatepark 記事を開きます。 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![記事のベーススタイルを更新しました](assets/client-side-libraries/updated-base-styles.png)

   これで、記事の更新されたスタイルが表示されます。 ブラウザーでキャッシュされた CSS ファイルをクリアするには、ハードリフレッシュを実行する必要がある場合があります。

   モックアップの方がずっと近づいてきた！

   >[!NOTE]
   >
   > 上記の手順で、ui.frontend コードを構築してAEMにデプロイすると、Maven ビルドがプロジェクトのルートからトリガーされた場合に、自動的に実行されます `mvn clean install -PautoInstallSinglePackage`.

## スタイルを変更する

次に、 `ui.frontend` モジュールを参照してください `npm run watch` ローカルのAEMインスタンスにスタイルを自動的にデプロイします。

1. 内 `ui.frontend` モジュールは次のファイルを開きます。 `ui.frontend/src/main/webpack/site/_variables.scss`.
1. を更新します。 `$brand-primary` 色変数：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   変更を保存します。

1. ブラウザーに戻り、AEMページを更新して更新内容を確認します。

   ![クライアント側ライブラリ](assets/client-side-libraries/style-update-brand-primary.png)

1. 変更を `$brand-primary` コマンドを使用して webpack ビルドをカラーで停止します。 `CTRL+C`.

>[!CAUTION]
>
> の使用 **ui.frontend** モジュールは、すべてのプロジェクトに必要とは限りません。 この **ui.frontend** モジュールは、さらに複雑になります。これらの高度なフロントエンドツール（Sass、webpack、npm など）の一部を使用する必要がない場合や使用したいと思わない場合は、不要な場合があります。

## ページとテンプレートの追加 {#page-inclusion}

次に、clientlib がAEM Page でどのように参照されているかを確認します。 Web 開発の一般的なベストプラクティスは、CSS をヘッダーに含めることです。HTML `<head>` と JavaScript を閉じる直前に `</body>` タグを使用します。

1. 次の場所にある記事ページテンプレートを参照します。 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 次をクリック： **ページ情報** アイコンとメニューでを選択します。 **ページポリシー** 開く **ページポリシー** ダイアログ。

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy.png)

   *ページ情報/ページポリシー*

1. 次のカテゴリに注意してください。 `wknd.dependencies` および `wknd.site` を参照してください。 デフォルトでは、ページポリシーを使用して設定した clientlib は、ページの先頭に CSS を、本文の末尾に JavaScript を含むように分割されます。 必要に応じて、clientlib JavaScript がページの先頭に読み込まれることを明示的に示すことができます。 これは `wknd.dependencies`.

   ![記事ページテンプレートメニューページポリシー](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > また、 `wknd.site` または `wknd.dependencies` を使用して、ページコンポーネントから直接 `customheaderlibs.html` または `customfooterlibs.html` 前に見たように、台本を `wknd.base` clientlib. テンプレートを使用すると、テンプレートごとに使用する clientlib を柔軟に選択できます。 例えば、選択したテンプレートでのみ使用される重い JavaScript ライブラリがある場合、

1. 次に移動： **LA スケートパークス** を使用して作成されたページ **記事ページテンプレート**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 次をクリック： **ページ情報** アイコンとメニューでを選択します。 **公開済みとして表示** をクリックして、AEM Editor の外部で記事ページを開きます。

   ![公開済みとして表示](assets/client-side-libraries/view-as-published-article-page.png)

1. ページのソースを表示 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 次の clientlib 参照が `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   clientlibs はプロキシを使用しています。 `/etc.clientlibs` endpoint. また、次の clientlib がページの下部に含まれていることもわかります。

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4 に続く場合、クライアント側ライブラリは自動的に縮小されません。 詳しくは、 [HTMLライブラリマネージャーで縮小を有効にする（推奨）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja#using-preprocessors).

   >[!WARNING]
   >
   >セキュリティ上の理由から、/apps パスは&#x200B;**** Dispatcher の filter セクション&#x200B;**にのみ使用すべきであるので、パブリッシュ側では、クライアントライブラリを /apps から提供**&#x200B;しない[](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)ことが重要となります。この [allowProxy プロパティ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) クライアントライブラリのを使用すると、CSS と JS が **/etc.clientlibs**.

### 次の手順 {#next-steps}

個々のスタイルを実装し、Experience Managerのスタイルシステムを使用してコアコンポーネントを再利用する方法について説明します。 [スタイルシステムを使用した開発](style-system.md) スタイルシステムを使用して、ブランド固有の CSS やテンプレートエディターの高度なポリシー設定でコアコンポーネントを拡張する方法について説明します。

で完成したコードを表示する [GitHub](https://github.com/adobe/aem-guides-wknd) または、Git ブランチのローカルのにコードを確認してデプロイします。 `tutorial/client-side-libraries-solution`.

1. のクローン [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) リポジトリ。
1. 以下を確認します。 `tutorial/client-side-libraries-solution` 分岐。

## その他のツールおよびリソース {#additional-resources}

### Webpack DevServer — 静的マークアップ {#webpack-dev-static}

前の 2 つの演習では、 **ui.frontend** モジュールを使用し、ビルドプロセスを通じて、最終的に、これらの変更がAEMに反映されていることを確認します。 次に、 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 対するフロントエンドスタイルを迅速に開発する **静的** HTML。

この手法は、ほとんどのスタイルとフロントエンドコードが、AEM環境に容易にアクセスできない専用のフロントエンド開発者によって実行されている場合に便利です。 また、この手法を使用すると、FED はHTMLに直接変更を加え、それをAEM開発者に渡して、コンポーネントとして実装できます。

1. 次の LA スケートパーク記事ページのページソースをコピーします。 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. IDE を再度開きます。 AEMからコピーしたマークアップを `index.html` 内 **ui.frontend** 下のモジュール `src/main/webpack/static`.
1. コピーしたマークアップを編集し、 **clientlib-site** および **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   webpack 開発サーバーがこれらのアーティファクトを自動的に生成するので、これらの参照を削除できます。

1. 新しいターミナルから webpack 開発サーバーを起動するには、次のコマンドを **ui.frontend** モジュール：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 新しいブラウザーウィンドウが次の場所に開きます： [http://localhost:8080/](http://localhost:8080/) 静的マークアップを使用

1. ファイルを編集 `src/main/webpack/site/_variables.scss` ファイル。 を `$text-color` ルールには次の情報が含まれます。

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   変更を保存します。

1. 変更が自動的にブラウザーに反映され、 [http://localhost:8080](http://localhost:8080).

   ![ローカル webpack 開発サーバーの変更](assets/client-side-libraries/local-webpack-dev-server.png)

1. 以下を確認します。 `/aem-guides-wknd.ui.frontend/webpack.dev.js` ファイル。 webpack-dev-server の起動に使用する Webpack 設定が含まれます。 パスをプロキシします `/content` および `/etc.clientlibs` ローカルで実行されているAEMインスタンスから。 これにより、画像や他の clientlib( **ui.frontend** コード ) が使用可能になります。

   >[!CAUTION]
   >
   > 静的マークアップの画像 src は、ローカルAEMインスタンス上のライブ画像コンポーネントを指しています。 画像のパスが変更された場合、AEMが起動されていない場合、またはブラウザーがローカルのAEMインスタンスにログインしていない場合は、画像が壊れて表示されます。 外部リソースに渡す場合は、画像を静的な参照で置き換えることもできます。

1. 以下が可能です。 **停止** コマンドラインから、次のように入力して webpack サーバーを `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** は、フロントエンド開発の高速化に使用できるオープンソースのコマンドラインツールです。 これは次の方法で動作します。 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/)、および [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

高レベルで **aemfed** は、 **ui.apps** モジュールを使用して、実行中のAEMインスタンスに直接同期します。 変更に基づき、ローカルブラウザーが自動更新されるので、フロントエンド開発がスピードアップします。また、Sling Log tracer と連携して、サーバー側のエラーを端末に直接自動的に表示するように設計されています。

もし、 **ui.apps** モジュール、HTL スクリプトの変更、カスタムコンポーネントの作成 **aemfed** は、使用する強力なツールです。 [完全なドキュメントは、こちらを参照してください。](https://github.com/abmaonline/aemfed).

### クライアント側ライブラリのデバッグ {#debugging-clientlibs}

様々な方法で **カテゴリ** および **埋め込み** 複数のクライアントライブラリを含める場合、トラブルシューティングが面倒になる可能性があります。 AEM はそのためにいくつかのツールを公開しています。最も重要なツールの 1 つは、 **クライアントライブラリの再構築** これにより、AEMはすべての LESS ファイルを再コンパイルし、CSS を生成します。

* [**ライブラリのダンプ**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEMインスタンスに登録されているクライアントライブラリをリストします。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**テスト出力**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — カテゴリに基づいて、clientlib インクルードの期待されるHTML出力をユーザーが確認できるようにします。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**ライブラリ依存関係の検証**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 見つからない依存関係または埋め込みカテゴリを強調表示します。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**クライアントライブラリの再構築**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - AEMに対し、クライアントライブラリを強制的に再構築するか、クライアントライブラリのキャッシュを無効にすることができます。 このツールは LESS を使用して開発する場合に有効です。生成された CSS を強制的に再コンパイルすることができます。 一般に、キャッシュを無効にしてから、ページの更新を実行する方がライブラリを再構築するよりも効果的です。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![クライアントライブラリを再構築](assets/client-side-libraries/rebuild-clientlibs.png)
