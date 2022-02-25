---
title: スタイルシステムを使用した開発
seo-title: Developing with the Style System
description: Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法について説明します。 このチュートリアルでは、スタイルシステムを使用して、ブランド固有の CSS とテンプレートエディターの高度なポリシー設定を使用してコアコンポーネントを拡張するための開発について説明します。
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 13%

---

# スタイルシステムを使用した開発 {#developing-with-the-style-system}

Experience Managerのスタイルシステムを使用して、個々のスタイルを実装し、コアコンポーネントを再利用する方法について説明します。 このチュートリアルでは、スタイルシステムを使用して、ブランド固有の CSS とテンプレートエディターの高度なポリシー設定を使用してコアコンポーネントを拡張するための開発について説明します。

## 前提条件 {#prerequisites}

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

また、 [クライアント側ライブラリとフロントエンドワークフロー](client-side-libraries.md) AEMプロジェクトに組み込まれているクライアント側ライブラリの基本と様々なフロントエンドツールについて理解するためのチュートリアルです。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの構築元となるベースラインコードを確認します。

1. 以下を確認します。 `tutorial/style-system-start` ～から分岐する [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `tutorial/style-system-solution`.

## 目的

1. スタイルシステムを使用して、ブランド固有の CSS をAEMコアコンポーネントに適用する方法を説明します。
1. BEM 表記と、BEM 表記を使用してスタイルを慎重にスコープ設定する方法について説明します。
1. 編集可能テンプレートを使用して詳細なポリシー設定を適用します。

## 作成する内容 {#what-you-will-build}

この章では、 [スタイルシステム機能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=ja) バリエーションを作成するには **タイトル** および **テキスト** 記事ページで使用されるコンポーネント。

![タイトルに使用できるスタイル](assets/style-system/styles-added-title.png)

*タイトルコンポーネントで使用できる下線スタイル*

## 背景 {#background}

開発者およびテンプレート編集者は、[スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html)を使用して、コンポーネントの複数の視覚的バリエーションを作成できます。次に、作成者はページを構成する際に、どのスタイルを使用するかを決めることができます。以降のすべてのチュートリアルでは、ローコードの手法でコアコンポーネントを使用しながら、スタイルシステムを使用して、いくつかの独自のスタイルを創出していきます。

スタイルシステムの基本的な考え方は、コンポーネントがどのように表示されるかについて、作成者が様々なスタイルを選択できるようにすることです。「スタイル」は、コンポーネントの外側の div に取り込まれた追加の CSS クラスに基づき実現されます。これらのスタイルクラスに基づき、CSS ルールがクライアントライブラリに追加され、コンポーネントの表示が変更されます。

以下を見つけることができます。 [スタイルシステムに関する詳細なドキュメントはこちら](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html?lang=ja). 素晴らしいものもあります [スタイルシステムについての技術ビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## 下線スタイル — タイトル {#underline-style}

この [タイトルコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) は、の下のプロジェクトにプロキシされています `/apps/wknd/components/title` の一部として **ui.apps** モジュール。 見出し要素 (`H1`, `H2`, `H3`) は既に **ui.frontend** モジュール。

この [WKND 記事デザイン](assets/pages-templates/wknd-article-design.xd) には、下線付きのタイトルコンポーネント用の独自のスタイルが含まれています。 スタイルシステムを使用すると、2 つのコンポーネントを作成したり、コンポーネントダイアログを変更する代わりに、下線のスタイルを作成者に追加するオプションを使用できます。

![下線スタイル — タイトルコンポーネント](assets/style-system/title-underline-style.png)

### タイトルポリシーを追加

コンテンツ作成者が特定のコンポーネントに適用する下線スタイルを選択できるように、タイトルコンポーネント用の新しいポリシーを追加します。 それには、AEM内のテンプレートエディターを使用します。

1. 次に移動： **記事ページ** テンプレートは次の場所にあります。 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. In **構造** モード、メイン **レイアウトコンテナ**&#x200B;を選択し、 **ポリシー** 横のアイコン **タイトル** 次に示すコンポーネント *許可されたコンポーネント*:

   ![タイトルポリシーの設定](assets/style-system/article-template-title-policy-icon.png)

1. 次の値を持つタイトルコンポーネントの新しいポリシーを作成します。

   *ポリシーのタイトル**: **WKND タイトル**

   *プロパティ* > *「スタイル」タブ* > *新しいスタイルを追加*

   **下線** : `cmp-title--underline`

   ![タイトルのスタイルポリシー設定](assets/style-system/title-style-policy.png)

   クリック **完了** をクリックして、タイトルポリシーの変更を保存します。

   >[!NOTE]
   >
   > 値 `cmp-title--underline` は、コンポーネントのマークアップの外側 div に CSS クラスをHTMLします。

### 下線のスタイルを適用

作成者が特定のタイトルコンポーネントに下線スタイルを適用するとき。

1. 次に移動： **ラスカテパークス** AEM Sites Editor の記事： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In **編集** モードで、タイトルコンポーネントを選択します。 次をクリック： **絵筆** アイコンをクリックし、 **下線** スタイル：

   ![下線のスタイルを適用](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > この時点で、 `underline` スタイルが実装されていません。 次の練習では、このスタイルを実装します。

1. 次をクリック： **ページ情報** アイコン/ **公開済みとして表示** をクリックして、AEMエディターの外部でページを調べます。
1. ブラウザーの開発者ツールを使用して、タイトルコンポーネントの周囲のマークアップに CSS クラスが含まれていることを確認します `cmp-title--underline` 外側の div に適用されます。

   ![下線クラスが適用された div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 下線スタイルの実装 — ui.frontend

次に、 **ui.frontend** プロジェクトのモジュール。 アドビは、 **ui.frontend** スタイルをプレビューするモジュール *前* AEMのローカルインスタンスにデプロイする

1. を開始します。 `watch` 内部からのプロセス **ui.frontend** モジュール：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   これにより、 `ui.frontend` 変更をAEMインスタンスに同期します。


1. IDE を返し、ファイルを開きます。 `_title.scss` 場所： `ui.frontend/src/main/webpack/components/_title.scss`.
1. をターゲットとする新しいルールを導入します。 `cmp-title--underline` クラス：

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
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
   >すべてのコアコンポーネントが準拠 **[BEM 表記](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. ベストプラクティスとしては、コンポーネントのデフォルトスタイルを作成する際は、外部の CSS クラスを指定することが推奨されます。また、HTML 要素ではなく、コアコンポーネントの BEM 記法で指定されたクラス名を指定することが推奨されます。

1. ブラウザーとAEMページに戻ります。 下線のスタイルが追加されます。

   ![webpack 開発サーバーで表示されるスタイルに下線を引く](assets/style-system/underline-implemented-webpack.png)

1. AEMエディターで、 **下線** スタイルを作成し、変更が視覚的に反映されていることを確認します。

## 引用ブロックのスタイル — テキスト {#text-component}

次に、同様の手順を繰り返して、 [テキストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=ja). テキストコンポーネントは、の下のプロジェクト内でプロキシ化されています。 `/apps/wknd/components/text` の一部として **ui.apps** モジュール。 段落要素のデフォルトスタイルは、既に **ui.frontend**.

この [WKND 記事デザイン](assets/pages-templates/wknd-article-design.xd) 引用符ブロック付きのテキストコンポーネントに固有のスタイルが含まれています。

![見積ブロックスタイル — テキストコンポーネント](assets/style-system/quote-block-style.png)

### テキストポリシーを追加

次に、テキストコンポーネントの新しいポリシーを追加します。

1. 次に移動： **記事ページテンプレート** 場所： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. In **構造** モード、メイン **レイアウトコンテナ**&#x200B;を選択し、 **ポリシー** 横のアイコン **テキスト** 次に示すコンポーネント *許可されたコンポーネント*:

   ![テキストポリシーの設定](assets/style-system/article-template-text-policy-icon.png)

1. テキストコンポーネントポリシーを次の値に更新します。

   *ポリシーのタイトル**: **コンテンツテキスト**

   *プラグイン* > *段落スタイル* > *段落スタイルを有効にする*

   *「スタイル」タブ* > *新しいスタイルを追加*

   **見積ブロック** : `cmp-text--quote`

   ![テキストコンポーネントポリシー](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![テキストコンポーネントポリシー 2](assets/style-system/text-policy-enable-quotestyle.png)

   クリック **完了** をクリックして、テキストポリシーの変更を保存します。

### 見積もりブロック・スタイルの適用

1. 次に移動： **ラスカテパークス** AEM Sites Editor の記事： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. In **編集** モードで、テキストコンポーネントを選択します。 コンポーネントを編集して、引用符要素を含めます。

   ![テキストコンポーネントの設定](assets/style-system/configure-text-component.png)

1. テキストコンポーネントを選択し、 **絵筆** アイコンをクリックし、 **見積ブロック** スタイル：

   ![見積もりブロック・スタイルの適用](assets/style-system/quote-block-style-applied.png)

1. ブラウザーの開発者ツールを使用して、マークアップを調べます。 クラス名が表示されます `cmp-text--quote` は、コンポーネントの outter div に追加されました。

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Quote Block Style の実装 — ui.frontend

次に、 **ui.frontend** プロジェクトのモジュール。

1. まだ実行していない場合は、を起動します。 `watch` 内部からのプロセス **ui.frontend** モジュール：

   ```shell
   $ npm run watch
   ```

1. ファイルを更新 `text.scss` 場所： `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
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
   > この場合、生のHTML要素は、スタイルのターゲットになります。 これは、テキストコンポーネントがコンテンツ作成者にリッチテキストエディターを提供するからです。 RTE コンテンツに対して直接スタイルを作成する場合は、慎重におこなう必要があります。また、スタイルを厳密にスコープ設定することがさらに重要です。

1. ブラウザに戻ると、Quote ブロックのスタイルが追加されます。

   ![見積ブロックのスタイルを表示](assets/style-system/quoteblock-implemented.png)

1. WebPack 開発サーバーを停止します。

## 固定幅 — コンテナ（ボーナス） {#layout-container}

コンテナコンポーネントは、記事ページテンプレートの基本構造を作成し、コンテンツ作成者がページにコンテンツを追加するためのドロップゾーンを提供するために使用されています。 コンテナはスタイルシステムを活用し、コンテンツ作成者がレイアウトをデザインするためのさらに多くのオプションを提供できます。

この **メインコンテナ** 記事ページテンプレートのには、2 つの作成可能なコンテナが含まれ、幅は固定されています。

![メインコンテナ](assets/style-system/main-container-article-page-template.png)

*記事ページテンプレートのメインコンテナ*.

のポリシー **メインコンテナ** デフォルトの要素を次のように設定します。 `main`:

![メインコンテナポリシー](assets/style-system/main-container-policy.png)

を **メインコンテナ** 固定値が **ui.frontend** モジュール `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

ターゲット設定の代わりに、 `main` HTML要素の場合、スタイルシステムを使用して **固定幅** スタイルをコンテナポリシーの一部として追加しました。 スタイルシステムを使用すると、ユーザーは **固定幅** および **流体の幅** コンテナ。

1. **ボーナスチャレンジ**  — 前の演習で学習したレッスンを使用し、スタイルシステムを使用して **固定幅** および **流体の幅** コンテナコンポーネントのスタイル。

## おめでとうございます。 {#congratulations}

おめでとうございます。記事ページはほぼ完全にスタイル設定され、AEMスタイルシステムを使用して実践的な体験を得ることができました。

### 次の手順 {#next-steps}

エンドツーエンドで [カスタムAEMコンポーネント](custom-component.md) これは、ダイアログで作成されたコンテンツを表示し、コンポーネントの HTL に入力するビジネスロジックをカプセル化する Sling モデルの開発を検討しています。

で完成したコードを表示する [GitHub](https://github.com/adobe/aem-guides-wknd) または、Git ブラッチ上のローカルのにコードを確認してデプロイします。 `tutorial/style-system-solution`.

1. のクローン [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) リポジトリ。
1. 以下を確認します。 `tutorial/style-system-solution` 分岐。
