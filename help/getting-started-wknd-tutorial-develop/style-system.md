---
title: スタイルシステムを使用した開発
seo-title: スタイルシステムを使用した開発
description: Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法を説明します。 このチュートリアルでは、スタイルシステムで、ブランド固有のCSSとテンプレートエディターの高度なポリシー設定を使用してコアコンポーネントを拡張するための開発について説明します。
sub-product: サイト
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
feature: コアコンポーネント、スタイルシステム
topic: コンテンツ管理、開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2003'
ht-degree: 12%

---


# スタイルシステムを使用した開発 {#developing-with-the-style-system}

Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法を説明します。 このチュートリアルでは、スタイルシステムで、ブランド固有のCSSとテンプレートエディターの高度なポリシー設定を使用してコアコンポーネントを拡張するための開発について説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

また、[クライアント側ライブラリとフロントエンドワークフロー](client-side-libraries.md)のチュートリアルを確認し、クライアント側ライブラリの基本原則とAEMプロジェクトに組み込まれた様々なフロントエンドツールについて理解することをお勧めします。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルが構築する基本行コードを調べます。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の`tutorial/style-system-start`ブランチを調べます。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)に常に表示できます。また、ブランチ`tutorial/style-system-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 目的

1. スタイルシステムを使用して、ブランド固有のCSSをAEMコアコンポーネントに適用する方法を説明します。
1. BEM表記の概要と、スタイルを慎重にスコープする方法について説明します。
1. 編集可能なテンプレートを使用して、ポリシー設定の詳細を適用します。

## 作成する内容 {#what-you-will-build}

この章では、[スタイルシステム機能](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)を使用して、記事ページで使用する&#x200B;**タイトル**&#x200B;と&#x200B;**テキスト**&#x200B;のコンポーネントのバリエーションを作成します。

![タイトルに使用できるスタイル](assets/style-system/styles-added-title.png)

*タイトルコンポーネントで使用できる下線スタイル*

## 背景 {#background}

開発者およびテンプレート編集者は、[スタイルシステム](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/components/style-system.html)を使用して、コンポーネントの複数の視覚的バリエーションを作成できます。次に、作成者はページを構成する際に、どのスタイルを使用するかを決めることができます。以降のすべてのチュートリアルでは、ローコードの手法でコアコンポーネントを使用しながら、スタイルシステムを使用して、いくつかの独自のスタイルを創出していきます。

スタイルシステムの基本的な考え方は、コンポーネントがどのように表示されるかについて、作成者が様々なスタイルを選択できるようにすることです。「スタイル」は、コンポーネントの外側の div に取り込まれた追加の CSS クラスに基づき実現されます。これらのスタイルクラスに基づき、CSS ルールがクライアントライブラリに追加され、コンポーネントの表示が変更されます。

[スタイルシステムに関する詳細なドキュメントは、](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html?lang=ja)を参照してください。 また、スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)を理解するための[テクニカルビデオもあります。

## 下線のスタイル — タイトル{#underline-style}

[タイトルコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html)が&#x200B;**ui.apps**&#x200B;モジュールの一部として`/apps/wknd/components/title`の下のプロジェクト内にプロキシ化されました。 見出し要素(`H1`、`H2`、`H3`...)のデフォルトのスタイルは、既に&#x200B;**ui.frontend**&#x200B;モジュールに実装されています。

[WKND記事デザイン](assets/pages-templates/wknd-article-design.xd)には、タイトルコンポーネントに対して一意のスタイルが含まれ、下線が引かれています。 2つのコンポーネントを作成したり、コンポーネントダイアログを修正する代わりに、スタイルシステムを使用して、下線スタイルを追加するオプションを作成できます。

![下線スタイル — タイトルコンポーネント](assets/style-system/title-underline-style.png)

### Inspectタイトルマークアップ

フロントエンド開発者として、コアコンポーネントのスタイル設定を行う最初の手順は、コンポーネントによって生成されるマークアップを理解することです。

1. 新しいブラウザーを開き、AEMコアコンポーネントライブラリサイトのタイトルコンポーネントを表示します。[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. 次に、Titleコンポーネントのマークアップを示します。

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   タイトルコンポーネントのBEM表記：

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. スタイルシステムは、コンポーネントを囲む外側のdivにCSSクラスを追加します。 したがって、対象とするマークアップは次のようになります。

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### 下線スタイルの実装 — ui.frontend

次に、プロジェクトの&#x200B;**ui.frontend**&#x200B;モジュールを使用してUnderlineスタイルを実装します。 **ui.frontend**&#x200B;モジュールに組み込まれているWebPack開発サーバーを使用して、*導入前のスタイル*&#x200B;をAEMのローカルインスタンスにプレビューします。

1. **ui.frontend**&#x200B;モジュール内で次のコマンドを実行して、webpackデベロッパーサーバーを開始します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   これにより、[http://localhost:8080](http://localhost:8080)でブラウザが開きます。

   >[!NOTE]
   >
   > 画像が壊れているように見える場合は、スタータープロジェクトがAEMのローカルインスタンス（ポート4502で実行）にデプロイされており、使用されるブラウザーもローカルのAEMインスタンスにログインしていることを確認してください。

   ![Webpack開発サーバー](assets/style-system/static-webpack-server.png)

1. IDEで、次の場所にあるファイル`index.html`を開きます。`ui.frontend/src/main/webpack/static/index.html`. これは、WebPack開発サーバーで使用される静的マークアップです。
1. `index.html`で、*cmp-title*&#x200B;をドキュメントで検索して、下線のスタイルを追加するTitleコンポーネントのインスタンスを探します。 「タイトル」コンポーネントを選択し、「壁のスケートパークから離れたバン」*（行218）というテキストを入力します。*&#x200B;周囲追加のdivに対するクラス`cmp-title--underline`:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. ブラウザに戻り、追加のクラスがマークアップに反映されていることを確認します。
1. **ui.frontend**&#x200B;モジュールに戻り、次の場所にあるファイル`title.scss`を更新します。`ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >ベストプラクティスとしては、スタイルをターゲットコンポーネントで使用する範囲に収めることが推奨されます。これにより、ページの他の領域が余分なスタイルの影響を受けることを回避できます。
   >
   >すべてのコアコンポーネントは、**[BEM表記](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**&#x200B;に従います。 ベストプラクティスとしては、コンポーネントのデフォルトスタイルを作成する際は、外部の CSS クラスを指定することが推奨されます。また、HTML 要素ではなく、コアコンポーネントの BEM 記法で指定されたクラス名を指定することが推奨されます。

1. もう一度ブラウザに戻ると、下線スタイルが追加されます。

   ![WebPack Devサーバーで表示されるスタイルに下線を引く](assets/style-system/underline-implemented-webpack.png)

1. WebPack開発サーバーを停止します。

### 権追加原政策

次に、Titleコンポーネントの新しいポリシーを追加して、コンテンツ作成者が特定のコンポーネントに適用する下線スタイルを選択できるようにする必要があります。 これは、AEMのテンプレートエディターを使用して行います。

1. Mavenのスキルを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 次の場所にある&#x200B;**記事ページ**&#x200B;テンプレートに移動します。[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. **構造**&#x200B;モードのメイン&#x200B;**レイアウトコンテナ**&#x200B;で、*許可されているコンポーネント*&#x200B;の下に表示されている&#x200B;**タイトル**&#x200B;コンポーネントの横の&#x200B;**ポリシー**&#x200B;アイコンを選択します。

   ![タイトルポリシーの構成](assets/style-system/article-template-title-policy-icon.png)

1. 次の値を持つTitleコンポーネントの新しいポリシーを作成します。

   *ポリシータイトル**: **WKNDタイトル**

   *プロパティ* / *スタイルタブ*  */追加新しいスタイル*

   **下線** :  `cmp-title--underline`

   ![タイトルのスタイルポリシー設定](assets/style-system/title-style-policy.png)

   「**完了**」をクリックして、タイトルポリシーの変更を保存します。

   >[!NOTE]
   >
   > 値`cmp-title--underline`は、**ui.frontend**&#x200B;モジュールで開発する際に、先ほどターゲットにしたCSSクラスと一致します。

### 下線のスタイルを適用する

最後に、作成者は、特定のタイトルコンポーネントに下線のスタイルを適用することを選択できます。

1. AEM Sitesの編集長の&#x200B;**La Skateparks**&#x200B;記事に移動します。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. **編集**&#x200B;モードで、タイトルコンポーネントを選択します。 **paintbrush**&#x200B;アイコンをクリックし、**下線**&#x200B;スタイルを選択します。

   ![下線のスタイルを適用する](assets/style-system/apply-underline-style-title.png)

   作成者は、スタイルのオン/オフを切り替えることができます。

1. **ページ情報**&#x200B;アイコン/**発行済みの表示**&#x200B;をクリックして、AEMエディターの外部のページを検査します。

   ![公開済みとして表示](assets/style-system/view-as-published.png)

   ブラウザー開発者ツールを使用して、Titleコンポーネントの周りのマークアップに、外側のdivに適用されたCSSクラス`cmp-title--underline`があることを確認します。

## 引用ブロックのスタイル — テキスト{#text-component}

次に、同様の手順を繰り返して、[テキストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)に一意のスタイルを適用します。 テキストコンポーネントは、**ui.apps**&#x200B;モジュールの一部として、`/apps/wknd/components/text`下のプロジェクトにプロキシされました。 段落要素のデフォルトのスタイルは、既に&#x200B;**ui.frontend**&#x200B;に実装されています。

[WKND記事デザイン](assets/pages-templates/wknd-article-design.xd)には、引用符ブロックを含むテキストコンポーネントの一意のスタイルが含まれています。

![引用ブロックスタイル — 文字コンポーネント](assets/style-system/quote-block-style.png)

### Inspect文字コンポーネントマークアップ

もう一度、Textコンポーネントのマークアップを調べます。

1. 次の場所で、テキストコンポーネントのマークアップを確認します。[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. 次に、Textコンポーネントのマークアップを示します。

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   テキストコンポーネントのBEM表記：

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. スタイルシステムは、コンポーネントを囲む外側のdivにCSSクラスを追加します。 したがって、対象とするマークアップは次のようになります。

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### Quote Blockスタイルの実装 — ui.frontend

次に、プロジェクトの&#x200B;**ui.frontend**&#x200B;モジュールを使用して、Quote Blockスタイルを実装します。

1. **ui.frontend**&#x200B;モジュール内で次のコマンドを実行して、webpackデベロッパーサーバーを開始します。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. IDEで、次の場所にあるファイル`index.html`を開きます。`ui.frontend/src/main/webpack/static/index.html`.
1. `index.html`で、テキスト&#x200B;*&quot;Jacob Wester&quot;*&#x200B;を検索して、テキストコンポーネントのインスタンスを探します（210行目）。 周囲追加のdivに対するクラス`cmp-text--quote`:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. 次の場所にあるファイル`text.scss`を更新します。`ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > この場合、生のHTML要素は、スタイルによってターゲット設定されます。 これは、テキストコンポーネントが、コンテンツ作成者向けのリッチテキストエディターを提供するためです。 RTEコンテンツに対して直接スタイルを作成する場合は、慎重に行う必要があり、スタイルを厳密にスコープすることがさらに重要です。

1. もう一度ブラウザに戻ると、次のようなQuoteブロックスタイルが追加されます。

   ![WebPack Devサーバーに表示される引用ブロックスタイル](assets/style-system/quoteblock-implemented-webpack.png)

1. WebPack開発サーバーを停止します。

### テ追加キストポリシー

次に、Textコンポーネント用の新しいポリシーを追加します。

1. Mavenのスキルを使用して、ローカルAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 次の場所にある&#x200B;**記事ページテンプレート**&#x200B;に移動します。[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html))。

1. **構造**&#x200B;モードのメイン&#x200B;**レイアウトコンテナ**&#x200B;で、*許可されているコンポーネント*&#x200B;の下に表示されている&#x200B;**テキスト**&#x200B;コンポーネントの横の&#x200B;**ポリシー**&#x200B;アイコンを選択します。

   ![テキストポリシーの構成](assets/style-system/article-template-text-policy-icon.png)

1. 次の値でTextコンポーネントポリシーを更新します。

   *ポリシータイトル**: **コンテンツテキスト**

   *プラグイン* / *段落スタイル* /段落スタイルを *有効にする*

   *[スタイル]タブ* > *新しいスタイル*

   **引用ブロック** :  `cmp-text--quote`

   ![テキストコンポーネントポリシー](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![テキストコンポーネントポリシー2](assets/style-system/text-policy-enable-quotestyle.png)

   「**完了**」をクリックして、変更をテキストポリシーに保存します。

### 見積もりブロックスタイルの適用

1. AEM Sitesの編集長の&#x200B;**La Skateparks**&#x200B;記事に移動します。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. **編集**&#x200B;モードで、テキストコンポーネントを選択します。 コンポーネントを編集して、引用要素を含めます。

   ![テキストコンポーネントの設定](assets/style-system/configure-text-component.png)

1. テキストコンポーネントを選択し、**paintbrush**&#x200B;アイコンをクリックし、**ブロックを引用**&#x200B;スタイルを選択します。

   ![見積もりブロックスタイルの適用](assets/style-system/quote-block-style-applied.png)

   作成者は、スタイルのオン/オフを切り替えることができます。

## 固定幅 —コンテナ（賞与） {#layout-container}

コンテナコンポーネントは、記事ページテンプレートの基本的な構造を作成し、コンテンツ作成者がページにコンテンツを追加するためのドロップゾーンを提供するために使用されています。 また、コンテナはスタイルシステムを活用して、コンテンツ作成者にレイアウトのデザインに関するより多くのオプションを提供できます。

記事ページテンプレートの&#x200B;**メインコンテナ**&#x200B;には、2つの作成者可能なコンテナが含まれ、固定された幅を持ちます。

![メインコンテナ](assets/style-system/main-container-article-page-template.png)

*記事ページテンプレートのメインコンテナ*。

**メインコンテナ**&#x200B;のポリシーは、デフォルトの要素を`main`に設定します。

![メインコンテナポリシー](assets/style-system/main-container-policy.png)

**メインコンテナ**&#x200B;を固定するCSSは、`ui.frontend/src/main/webpack/site/styles/container_main.scss`の&#x200B;**ui.frontend**&#x200B;モジュールに設定されます。

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

`main` HTML要素をターゲットにする代わりに、スタイルシステムを使用して、コンテナポリシーの一部として&#x200B;**固定幅**&#x200B;スタイルを作成できます。 スタイルシステムでは、**固定幅**&#x200B;と&#x200B;**流体幅**&#x200B;のコンテナを切り替えるオプションをユーザーに提供できます。

1. **ボーナスチャレンジ** ：前の練習で学んだレッスンを使用し、スタイルシステムを使用して、コンテナコンポーネントの **固定** 幅 **流体** 幅スタイルを実装します。

## これで完了です! {#congratulations}

記事ページのスタイルはほぼ完成し、AEM Style Systemを使用した実際の操作を体験できました。

### 次の手順 {#next-steps}

ダイアログで作成されたコンテンツを表示する[カスタムAEMコンポーネント](custom-component.md)をエンドツーエンドで作成する手順を説明します。また、Slingモデルの開発で、コンポーネントのHTLを埋め込むビジネスロジックをカプセル化します。

[GitHub](https://github.com/adobe/aem-guides-wknd)上の完了したコードを表示するか、Gitブラック`tutorial/style-system-solution`上のローカルにコードを確認して展開します。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。
1. `tutorial/style-system-solution`ブランチを調べます。
