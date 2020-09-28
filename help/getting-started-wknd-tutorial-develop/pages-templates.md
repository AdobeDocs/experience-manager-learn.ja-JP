---
title: AEM Sitesの使い始めに — ページとテンプレート
seo-title: AEM Sitesの使い始めに — ページとテンプレート
description: 基本ページコンポーネントと編集可能なテンプレートとの関係について説明します。 コアコンポーネントがプロジェクト内でどのようにプロキシ化されるかを理解し、Adobe XDのモックアップに基づいて、適切に構成された記事ページテンプレートを作成するための、編集可能なテンプレートの高度なポリシー設定を学びます。
sub-product: サイト
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 3%

---


# ページとテンプレート {#pages-and-template}

この章では、基本ページコンポーネントと編集可能なテンプレートとの関係について説明します。 AdobeXDのモックアップに基づいて、スタイル設定されていない記事テンプレートを作成 [します](https://www.adobe.com/products/xd.html)。 テンプレートの構築の過程で、コアコンポーネントと編集可能なテンプレートの高度なポリシー設定について説明します。

## 前提条件 {#prerequisites}

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### スタータープロジェクト

チュートリアルが構築する基本行コードを調べます。

1. github.com/adobe/aem-guides-wknd [リポジトリをコピーします](https://github.com/adobe/aem-guides-wknd) 。
1. ブランチをチェックアウトし `pages-templates/start` ます。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Mavenのスキルを使用して、ローカルAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `pages-templates/solution`ます。

## 目的

1. Inspectは、Adobe XDで作成されたページデザインで、コアコンポーネントにマッピングします。
1. 編集可能なテンプレートの詳細と、ページコンテンツの詳細な制御を強制するためにポリシーを使用する方法を理解します。
1. テンプレートとページのリンク方法

## 作成する内容 {#what-you-will-build}

チュートリアルのこの部分では、新しい記事ページの作成と共通の構造との整合に使用できる新しい記事ページテンプレートを作成します。 記事ページテンプレートは、AdobeXDで作成されたデザインとUIキットに基づいて作成されます。 この章では、テンプレートの構造またはスケルトンの作成にのみ焦点を当てます。 スタイルは実装されませんが、テンプレートとページは機能します。

![記事のページデザインとスタイル設定解除バージョン](assets/pages-templates/what-you-will-build.png)

## Adobe XDとのUIの計画 {#adobexd}

ほとんどの場合、モックアップと静的デザインを含む新しいWebサイト開始の作成を計画します。 [Adobe XD](https://www.adobe.com/products/xd.html) は、ユーザーエクスペリエンスを構築するデザインツールです。 次に、UIキットとモックアップを調べ、記事ページテンプレートの構造の計画に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

WKND記事デザインファイル [をダウンロードします](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)。

## エクスペリエンスフラグメントを含むヘッダーとフッターの作成 {#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する場合、一般的に [エクスペリエンスフラグメントを使用します](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1つの参照可能なコンポーネントを作成できます。 エクスペリエンスフラグメントには、複数サイトの管理をサポートする利点があり、ロケールごとに異なるヘッダー/フッターを管理できます。

次に、ヘッダーとフッターとして使用する予定のエクスペリエンスフラグメントを更新し、WKNDロゴを追加します。

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> エクスペリエンスフラグメントの外観は、ビデオとは異なりますか？ これらを削除して、スタータープロジェクトコードベースを再インストールしてみてください。

以下は、上記のビデオで実行される高レベルの手順です。

1. WKNDの暗いロゴを含めるには、http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.htmlにあるエクスペリエンスフラグメントヘッダー [を更新します](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) 。

   ![WKNDダークロゴ](assets/pages-templates/wknd-logo-dk.png)

   *WKNDダークロゴ*

1. WKNDライトロゴを含めるには、http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html [にあるエクスペリエンスフラグメントヘッダーを更新します](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) 。

   ![WKNDライトロゴ](assets/pages-templates/wknd-logo-light.png)

   *WKNDライトロゴ*

## 記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これは新しいページを作成するための基本として使用されます。テンプレートは、結果のページ、初期コンテンツ、許可されるコンポーネントの構造を定義します。

There are 3 main areas of [Editable Templates](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **構造** — テンプレートの一部であるコンポーネントを定義します。 コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ** — テンプレートを開始するコンポーネントを定義します。これらのコンポーネントは、コンテンツ作成者が編集または削除できます。
1. **ポリシー** — コンポーネントの動作方法と作成者が使用できるオプションに関する設定を定義します。

次に、記事ページテンプレートを作成します。 これは、AEMのローカルインスタンスで発生します。

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

以下は、上記のビデオで実行される高レベルの手順です。

1. WKND Sites Templateフォルダーに移動します。 **ツール** / **一般** / **テンプレート****/WKNDサイト**
1. 「 **WKNDサイトの空のページ** 」テンプレートタイプを使用し、タイトルを「 **記事ページテンプレート」に設定して新しいテンプレートを作成します**
1. 構 **造** モードで、次の要素を含めるようにテンプレートを設定します。

   * エクスペリエンスフラグメントヘッダー
   * 画像
   * パンくず
   * コンテナ — 幅が8列のデスクトップ、幅が12列のタブレット、モバイル
   * コンテナ — 幅が4列、幅が12列のタブレット、モバイル
   * エクスペリエンスフラグメントフッター

   ![構造モードの記事ページのテンプレート](assets/pages-templates/article-page-template-structure.png)

   *構造 — 記事ページのテンプレート*

1. **初期コンテンツ** モードに切り替え、以下のコンポーネントをスターターコンテンツとして追加します。

   * **メインコンテナ**
      * タイトル — H1のデフォルトサイズ
      * タイトル — *「作成者名* 」（サイズはH4）
      * テキスト — 空
   * **サイドコンテナ**
      * タイトル — *&quot;Share this Story&quot;* with a Size H5
      * ソーシャルメディア共有
      * 区切り文字
      * ダウンロード
      * リスト

   ![初期コンテンツモードの記事ページテンプレート](assets/pages-templates/article-page-template-initialcontent.png)

   *初期コンテンツ — 記事ページのテンプレート*

1. 「 **初期ページプロパティ** 」を更新して、 **Facebook** と **Pinterestの両方でユーザー共有を有効にします**。
1. 画像を簡単に識別できるように、 **記事ページテンプレートのプロパティに画像をアップロードします** 。

   ![記事ページテンプレートのサムネール](assets/pages-templates/article-page-template-thumbnail.png)

   *記事ページテンプレートのサムネール*

1. WKND Site Templatesフォルダーの **Article Page Template** ( [記事ページテンプレート)を有効にします](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates)。

## 記事ページの作成

テンプレートが完成したら、そのテンプレートを使用して新しいページを作成します。

1. 次のzipパッケージWKND-PagesTemplates- [DAM-Assets.zipをダウンロードし](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip) 、 [CRX Package Managerを使用してインストールします](http://localhost:4502/crx/packmgr/index.jsp)。

   上記のパッケージでは、後の手順で記事ページに入力するために、下に複数 `/content/dam/wknd/en/magazine/la-skateparks` の画像とアセットをインストールします。

   *上記のパッケージ内の画像とアセットは、[Unsplash.comの許可を得て、無料でライセンスを取得できます](https://unsplash.com/)。*

   ![DAMアセットのサンプル](assets/pages-templates/sample-assets-la-skatepark.png)

1. WKND **/** US **/en** en **（米国）の下にMagazineという名前の新しい******&#x200B;ページを作成します。 「 **コンテンツページ** 」テンプレートを使用します。

   このページでは、サイトに構造が追加され、実行中の階層リンクコンポーネントを確認できます。

1. 次に、 **WKND** / **US** / **en** / **** Magazine notedの下に新しいページを作成します。 「 **記事ページ** 」テンプレートを使用します。 LA Skateparksのタイトルは **Ultimate guide** 、 **guide-la-skateparksの名前を使用します**。

   ![最初に作成した記事ページ](assets/pages-templates/create-article-page-nocontent.png)

1. AdobeXD [部分を含む](#adobexd) UI Planningで検査されたモックアップと一致させるために、ページにコンテンツを入力します。 記事のサンプルテキストはこちらから [ダウンロードできます](assets/pages-templates/la-skateparks-copy.txt)。 次のようなものを作成できるはずです。

   ![入力された記事ページ](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > ページの上部にある画像コンポーネントは編集できますが、削除することはできません。 階層リンクコンポーネントはページに表示されますが、編集または削除はできません。

## ノード構造のInspect {#node-structure}

この時点で、記事のページのスタイルは明確に解除されます。 ただし、基本構造は定められている。 次に、テンプレートの役割と、コンテンツのレンダリングを担当するページコンポーネントの役割を理解するために、記事ページのノード構造を見てみます。

これは、ローカルのAEMインスタンスでCRXDE-Liteツールを使用して行うことができます。

1. [CRXDE-Liteを開き](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 、ツリーナビゲーションを使用してに移動し `/content/wknd/us/en/magazine/guide-la-skateparks`ます。

1. ページの下の `jcr:content` ノードをクリックし、 `la-skateparks` プロパティを表示します。

   ![JCRコンテンツプロパティ](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   前の手順で作成した記事ペ `cq:template`ージテンプレートの値 `/conf/wknd/settings/wcm/templates/article-page`（を参照）に注目してください。

   また、の値（を指す値） `sling:resourceType`にも注意してくだ `wknd/components/structure/page`さい。 これは、AEMプロジェクトのアーキタイプによって作成されるページコンポーネントで、テンプレートに基づいてページをレンダリングします。

1. ノード階層の下の `jcr:content` ノードを展開し、ノード階層の下の `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 表示を展開します。

   ![JCRコンテンツLAスケートパークス](assets/pages-templates/page-jcr-structure.png)

   各ノードを、オーサリングされたコンポーネントに緩やかにマッピングできるはずです。 プリフィックスが付いたノードを検査することで使用される様々なレイアウトコンテナを特定できるかどうかを確認 `responsivegrid`します。

1. 次に、のページコンポーネントを調べ `/apps/wknd/components/structure/page`ます。 CRXDE Lite内のコンポーネントプロパティの表示:

   ![ページコンポーネントプロパティ](assets/pages-templates/page-component-properties.png)

   ページコンポーネントは、 **structureという名前のフォルダーの下にあります**。 これは、テンプレートエディターの構造モードに対応する規則で、ページコンポーネントが作成者が直接操作するものではないことを示すために使用されます。

   HTLスクリプトは2つのみで、ページコンポーネント `customfooterlibs.html` の下 `customheaderlibs.html` にあります。 では、このコンポーネントはどのようにページをレンダリングするのか？

   の `sling:resourceSuperType` プロパティと値をメモしておき `core/wcm/components/page/v2/page`ます。 このプロパティーを使用すると、WKNDのページコンポーネントがコアコンポーネントページコンポーネントのすべての機能を継承できます。 これは、[プロキシコンポーネントパターン](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)と呼ばれるものの最初の例です。詳しくは、[こちら](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html)を参照してください。。

1. WKNDコンポーネント内の別のコンポーネント( `Breadcrumb` コンポーネントは次の場所に配置)をInspectします。 `/apps/wknd/components/content/breadcrumb`. 同じ `sling:resourceSuperType` プロパティが見つかりますが、今回はを指していることに注意してくだ `core/wcm/components/breadcrumb/v2/breadcrumb`さい。 これは、Proxyコンポーネントパターンを使用してCoreコンポーネントを含める別の例です。 実際、WKNDコードベースのコンポーネントはすべて、AEMコアコンポーネントのプロキシです（当社の有名なHelloWorldコンポーネントを除く）。 ベストプラクティスは、カスタムコードを記述する *前に、コアコンポーネントの機能をできるだけ多く使用してみること* です。

1. 次に、CRXDE Liteを `/apps/core/wcm/components/page/v2/page` 使用して、コアコンポーネントページを調べます。

   ![コアコンポーネントページ](assets/pages-templates/core-page-component-properties.png)

   このページの下には、さらに多くのスクリプトが含まれています。 コアコンポーネントページには多くの機能が含まれています。 この機能は、メンテナンスと読みやすさを容易にするために、複数のスクリプトに分割されています。 を開き、次を探すことで、HTLスクリプトを含めるかどうかをトレースでき `page.html` ま `data-sly-include`す。

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   HTLを複数のスクリプトに分割するもう1つの理由は、プロキシコンポーネントが、カスタムのビジネスロジックを実装する個々のスクリプトを上書きできるようにすることです。 HTLスクリプト `customfooterlibs.html``customheaderlibs.html`と、は、明示的な目的で作成され、プロジェクトの実装によって上書きされます。

   編集可能なテンプレートが [コンテンツページのレンダリングに与える影響について詳しくは、この記事を参照してください](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages)。

1. 別のコアコンポーネント（の階層リンクなど）をInspect `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`します。 スクリプトを表示して、階層リンクコンポーネントのマークアップが最終的にどのように生成されるかを理解します。 `breadcrumb.html`

## ソース管理への設定の保存 {#configuration-persistence}

多くの場合、特にAEMプロジェクトの最初に、テンプレートや関連するコンテンツポリシーなどの設定を保持してソース管理を行うことが重要です。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性を確保できます。 プロジェクトが一定の成熟度に達したら、テンプレートの管理方法をパワーユーザーの特別なグループに変えることができます。

ここでは、テンプレートを他のコードの部分と同様に扱い、 **記事ページテンプレート** （下）をプロジェクトの一部として同期します。 これまで、AEMプロジェクトのコードをAEMのローカルインスタンスに **プッシュしました** 。 記 **事ページテンプレート** は、AEMのローカルインスタンスで直接作成されたものなので、テンプレートを **** 取り込むか、AEMプロジェクトに読み込む必要があります。 この目的のため、 **ui.content** モジュールはAEMプロジェクトに含まれています。

次のいくつかの手順はEclipse IDEを使用して行われますが、AEMのローカルインスタンスからコンテンツを **取り込む** 、またはインポートするように設定した任意のIDEを使用して行うことができます。

1. Eclipse IDEで、サーバーにAEMのローカルインスタンスへの接続AEM開発者ツールプラグインが起動済みであること、および **ui.content** モジュールがサーバー設定に追加されていることを確認します。

   ![Eclipseサーバー接続](assets/pages-templates/eclipse-server-started.png)

1. プロジェクトエクスプローラで **ui.content** モジュールを展開します。 フォルダー(小さいグロ `src` ブのアイコンが表示されているフォルダー)を展開し、に移動し `/conf/wknd/settings/wcm/templates`ます。

1. [!UICONTROL ノードを右クリックし] 、 `templates` 「サーバーから **読み込み…**:」を選択します。

   ![Eclipseインポートテンプレート](assets/pages-templates/eclipse-import-templates.png)

   「リポジトリから **インポート** 」ダイアログを確認し、「 **完了」をクリックします**。 これで、フォルダの `article-page-template` 下にが表示され `templates` ます。

1. コンテンツを読み込む手順を繰り返しますが、にある **ポリシー** ノードを選択し `/conf/wknd/settings/wcm/policies`ます。

   ![Eclipseインポートポリシー](assets/pages-templates/policies-article-page-template.png)

1. フ `filter.xml` ァイルをInspectに配置し `src/main/content/META-INF/vault/filter.xml`ます。

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   この `filter.xml` ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルター `mode="merge"` に、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示すが、 コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントでコンテンツが上書きされないこ **とが重要です** 。 フィルタ要素の操作の詳細については、 [FileVaultのドキュメント](https://jackrabbit.apache.org/filevault/filter.html) を参照してください。

   各モジュール `ui.content/src/main/content/META-INF/vault/filter.xml` で管理され `ui.apps/src/main/content/META-INF/vault/filter.xml` る各ノードを比較し、理解します。

   >[!WARNING]
   >
   > WKNDリファレンスサイトで一貫したデプロイを行うために、プロジェクトの一部のブランチがセットアップされ、JCRの変更が上書きさ `ui.content` れます。 これは、特定のポリシーに対してコード/スタイルが書き込まれるので、ソリューションの分岐などに対して設計されています。

## バリデーターが{#congratulations}

新しいテンプレートとページがAdobe Experience Manager Sitesに作成されました。

### 次の手順 {#next-steps}

この時点で、記事のページのスタイルは明確に解除されます。 CSSとJavaScriptを含めて、サイトにグローバルスタイルを適用し、専用のフロントエンドビルドを統合するためのベストプラクティスについては、 [クライアント側ライブラリとフロントエンドワークフロー](client-side-libraries.md) のチュートリアルに従ってください。

GitHubに完了したコードを表示する [か](https://github.com/adobe/aem-guides-wknd) 、コードをローカルのGitブラッチに確認してデプロイ `pages-templates/solution`します。

1. github.com/adobe/aem-wknd-guides [リポジトリをコピーします](https://github.com/adobe/aem-guides-wknd) 。
1. ブランチをチェックアウトし `pages-templates/solution` ます。
