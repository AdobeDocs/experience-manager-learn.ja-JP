---
title: AEM Sites の基本を学ぶ - ページとテンプレート
description: ベースページコンポーネントと編集可能なテンプレートとの関係について説明します。コアコンポーネントがプロジェクト内でプロキシ化される方法を理解します。Adobe XD のモックアップに基づいて、適切に構造化された記事ページテンプレートを作成するための、編集可能なテンプレートの詳細ポリシー設定について説明します。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '2855'
ht-degree: 100%

---

# ページとテンプレート {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

この章では、ベースページコンポーネントと編集可能なテンプレートとの関係を探索します。[Adobe XD](https://helpx.adobe.com/jp/support/xd.html) の一部のモックアップに基づいて、スタイルが設定されていない記事テンプレートを作成する方法について説明します。テンプレートを作成するプロセスでは、コアコンポーネントと編集可能なテンプレートの詳細ポリシー設定について説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)を設定するために必要なツールや説明を確認します。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの作成元となるベースラインコードをチェックアウトします。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の `tutorial/pages-templates-start` ブランチをチェックアウトします。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

いつでも、完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) で確認したり、ブランチ `tutorial/pages-templates-solution` に切り替えてローカルにチェックアウトしたりできます。

## 目的

1. Adobe XD で作成したページデザインを検査し、コアコンポーネントにマッピングします。
1. 編集可能なテンプレートの詳細と、ポリシーを使用してページコンテンツを詳細に制御する方法を理解します。
1. テンプレートとページのリンク方法を学びます

## 作ろうとしているもの {#what-build}

チュートリアルのこの部分では、記事ページの作成に使用でき、共通の構造に合わせて新しい記事ページテンプレートを作成します。記事ページテンプレートは、Adobe XD で作成したデザインと UI キットに基づいています。この章では、テンプレートの構造またはスケルトンの構築にのみ焦点を当てています。スタイルは実装されていませんが、テンプレートとページは機能しています。

![記事ページデザインとスタイルが設定されていないバージョン](assets/pages-templates/what-you-will-build.png)

## Adobe XD を使用した UI の計画 {#adobexd}

通常、新しい web サイトの計画は、モックアップと静的なデザインから始まります。[Adobe XD](https://helpx.adobe.com/jp/support/xd.html) は、ユーザーエクスペリエンスを作成するデザインツールです。次に、記事ページテンプレートの構造を計画するのに役立つ UI キットとモックアップを探索します。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**[WKND 記事デザインファイル](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)をダウンロードします**。

>[!NOTE]
>
> カスタムプロジェクトの開始点として、汎用の [AEM コアコンポーネント UI キットも使用できます](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=ja)。

## 記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これはページを作成するための基本として使用されます。テンプレートは、作成されるページの構造、初期コンテンツおよび許可されるコンポーネントを定義します。

[編集可能なテンプレート](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=ja)には、主に次の 3 つの領域があります。

1. **構造** - テンプレートの一部であるコンポーネントを定義します。コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ** - テンプレート開始時のコンポーネントを定義します。コンテンツ作成者はこれらを編集／削除できます
1. **ポリシー** - コンポーネントの動作、および作成者が利用可能なオプションに関する設定を定義します。

次に、AEM でモックアップの構造に合ったテンプレートを作成します。これは、AEM のローカルインスタンスで発生します。以下のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上記のビデオの大まかな手順：

### 構造の設定

1. **ページテンプレートタイプ**&#x200B;を使用して、**記事ページ**&#x200B;という名前のテンプレートを作成します。
1. **構造**&#x200B;モードに切り替えます。
1. テンプレートの上部に、**ヘッダー**&#x200B;として機能する&#x200B;**エクスペリエンスフラグメント**&#x200B;コンポーネントを追加します。
   * コンポーネントが `/content/experience-fragments/wknd/us/en/site/header/master` を指すように設定します。
   * ポリシーを&#x200B;**ページヘッダー**&#x200B;に設定して、**デフォルトの要素**&#x200B;が `header` に設定されているようにしてください。次の章では、`header` 要素のターゲット設定を CSS で指定します。
1. テンプレートの最下部で、**フッター**&#x200B;として機能する&#x200B;**エクスペリエンスフラグメント**&#x200B;を追加します。
   * コンポーネントが `/content/experience-fragments/wknd/us/en/site/footer/master` を指すように設定します。
   * ポリシーを&#x200B;**ページフッター**&#x200B;に設定して、**デフォルトの要素**&#x200B;が `footer` に設定されているようにしてください。次の章では、`footer` 要素のターゲット設定を CSS で指定します。
1. テンプレートが最初に作成されたときに含まれていた&#x200B;**メイン**&#x200B;コンテナをロックします。
   * ポリシーを&#x200B;**メインページ**&#x200B;に設定して、**デフォルトの要素** が `main` に設定されているようにしてください。次の章では、`main` 要素のターゲット設定を CSS で指定します。
1. **画像**&#x200B;コンポーネントを&#x200B;**メイン**&#x200B;コンテナに追加します。
   * **画像**&#x200B;コンポーネントをロック解除します。
1. メインコンテナの&#x200B;**画像**&#x200B;コンポーネントの下に&#x200B;**パンくず**&#x200B;コンポーネントを追加します。
   * **パンくず**&#x200B;コンポーネントに&#x200B;**記事ページ - パンくず**&#x200B;という名前のポリシーを作成します。**ナビゲーション開始レベル**&#x200B;を **4** に設定します。
1. **コンテナ**&#x200B;コンポーネントを&#x200B;**パンくず**&#x200B;コンポーネントの下かつ&#x200B;**メイン**&#x200B;コンテナの内側に追加します。これは、テンプレートの&#x200B;**コンテンツコンテナ**&#x200B;として機能します。
   * **コンテンツ**&#x200B;コンテナをロック解除します。
   * ポリシーを&#x200B;**ページコンテンツ**&#x200B;に設定します。
1. 別の&#x200B;**コンテナ**&#x200B;コンポーネントを&#x200B;**コンテンツコンテナ**&#x200B;の下に追加します。これはテンプレートの&#x200B;**サイドパネル**&#x200B;コンテナとして機能します。
   * **サイドパネル**&#x200B;コンテナをロック解除
   * **記事ページ - サイドレール**&#x200B;という名前のポリシーを作成します。
   * **WKND Sites プロジェクト - コンテンツ**&#x200B;の下の&#x200B;**許可されたコンポーネント**&#x200B;を設定して、**ボタン**、**ダウンロード**、**画像**、**リスト**、**セパレーター**、**ソーシャルメディア共有**、**テキスト**、**タイトル**&#x200B;を含めます。
1. ページルートコンテナのポリシーを更新します。これは、テンプレートの最も外側にあるコンテナです。ポリシーを&#x200B;**ページルート**&#x200B;に設定します。
   * **コンテナ設定**&#x200B;で、**レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定します。
1. **コンテンツコンテナ**&#x200B;をレイアウトモードにします。ハンドルを右から左にドラッグし、コンテナを 8 列幅に縮小します。
1. **サイドレールコンテナ**&#x200B;のレイアウトモードを有効にします。ハンドルを右から左にドラッグし、コンテナを 4 列幅に縮小します。次に、左側のハンドルを左から右へ 1 列ドラッグしてコンテナを 3 列幅にし、**コンテンツコンテナ**&#x200B;の間に 1 列の間隔を残します。
1. モバイルエミュレーターを開き、モバイルブレークポイントに切り替えます。レイアウトモードを再度有効にし、**コンテンツコンテナ**&#x200B;と&#x200B;**サイドパネルコンテナ**&#x200B;をページの全幅にします。これにより、モバイルのブレークポイントにコンテナが垂直に積み重なります。
1. **コンテンツコンテナ**&#x200B;で&#x200B;**テキスト**&#x200B;コンポーネントのポリシーを更新します。
   * ポリシーを&#x200B;**コンテンツテキスト**&#x200B;に設定します。
   * **プラグイン**／**段落スタイル**&#x200B;で、**段落スタイルを有効にする**&#x200B;にチェックを入れ、**Quote Block** が有効になっていることを確認します。

### 初期コンテンツ設定

1. **初期コンテンツ**&#x200B;モードに切り替えます。
1. **タイトル**&#x200B;コンポーネントを&#x200B;**コンテンツコンテナ**&#x200B;に追加します。これは記事のタイトルとして機能します。空のままにすると、現在のページのタイトルが自動的に表示されます。
1. 最初のタイトルコンポーネントの下に、2 番目の&#x200B;**タイトル**&#x200B;コンポーネントを追加します。
   * 「By Author」というテキストを使用してコンポーネントを設定します。これはテキストのプレースホルダーです。
   * タイプを `H4` に設定します。
1. **By Author** タイトルコンポーネントの下に、**テキスト**&#x200B;コンポーネントを追加します。
1. **サイドパネルコンテナ**&#x200B;に&#x200B;**タイトル**&#x200B;コンポーネントを追加します。
   * 「このストーリーを共有」というテキストを使用してコンポーネントを設定します。
   * タイプを `H5` に設定します。
1. **このストーリーを共有**&#x200B;タイトルコンポーネントの下に、**ソーシャルメディア共有**&#x200B;コンポーネントを追加します。
1. **ソーシャルメディア共有**&#x200B;コンポーネントの下に&#x200B;**セパレーター**&#x200B;コンポーネントを追加します。
1. **セパレーター**&#x200B;コンポーネントの下に&#x200B;**ダウンロード**&#x200B;コンポーネントを追加します。
1. **ダウンロード**&#x200B;コンポーネントの下に&#x200B;**リスト**&#x200B;コンポーネントを追加します。
1. テンプレートの&#x200B;**初期ページプロパティ**&#x200B;を更新します。
   * **ソーシャルメディア**／**ソーシャルメディア共有**&#x200B;で、**Facebook** と **Pinterest** にチェックを入れます。

### テンプレートを有効にしてサムネールを追加

1. [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd) に移動して、テンプレートコンソールでテンプレートを表示します。
1. 記事ページテンプレートを&#x200B;**有効**&#x200B;にします。
1. 記事ページテンプレートのプロパティを編集し、次のサムネールをアップロードして、記事ページテンプレートを使用して作成したページを素早く特定します。

   ![記事ページテンプレートサムネール](assets/pages-templates/article-page-template-thumbnail.png)

## エクスペリエンスフラグメントを使用したヘッダーとフッターの更新 {#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する際の一般的な方法は、[エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja)を使用することです。エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1 つの参照可能なコンポーネントを作成できます。エクスペリエンスフラグメントには、マルチサイト管理と[ローカリゼーション](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=ja)をサポートするという利点があります。

AEM プロジェクトアーキタイプがヘッダーとフッターを生成しました。次に、モックアップと一致するようにエクスペリエンスフラグメントを更新します。以下のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上記のビデオの大まかな手順：

1. サンプルコンテンツパッケージ **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)** をダウンロードします。
1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp) でパッケージマネージャーを使用して、コンテンツパッケージをアップロードしてインストールします。
1. Web バリエーションテンプレートを更新します。これは、[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html) でエクスペリエンスフラグメントに使用されるテンプレートです。 
   * テンプレートの&#x200B;**コンテナ**&#x200B;コンポーネントのポリシーを更新します。
   * ポリシーを **XF ルート**&#x200B;に設定します。
   * **許可されたコンポーネント**&#x200B;で、コンポーネントグループ **WKND Sites プロジェクト - 構造**&#x200B;を選択し、**言語ナビゲーション**、**ナビゲーション**、**クイック検索**&#x200B;コンポーネントを含めます。

### ヘッダーエクスペリエンスフラグメントの更新

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) でヘッダーをレンダリングするエクスペリエンスフラグメントを開きます。
1. フラグメントのルート&#x200B;**コンテナ**&#x200B;を設定します。これが一番外側の **コンテナ**&#x200B;です。
   * **レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定します。
1. **コンテナ**&#x200B;の上部に **WKND ダークロゴ**&#x200B;を画像として追加します。ロゴは前の手順でインストールしたパッケージに含まれています。
   * **WKND ダークロゴ**&#x200B;のレイアウトを **2** 列の幅に変更します。ハンドルを右から左にドラッグします。
   * ロゴの&#x200B;**代替テキスト**&#x200B;を「WKND ロゴ」に設定します。
   * ロゴが `/content/wknd/us/en` ホームページに&#x200B;**リンク**&#x200B;するように設定します。
1. 既にページに配置されている&#x200B;**ナビゲーション**&#x200B;コンポーネントを設定します。
   * **ルートレベルを除外**&#x200B;を **1** に設定します。
   * **ナビゲーション構造の深さ**&#x200B;を **1** に設定します。
   * **ナビゲーション**&#x200B;コンポーネントのレイアウトを変更して、**8** 列幅にします。ハンドルを右から左にドラッグします。
1. **言語ナビゲーション**&#x200B;コンポーネントを削除します。
1. **検索**&#x200B;コンポーネントのレイアウトを変更して **2** 列幅にします。ハンドルを右から左にドラッグします。すべてのコンポーネントが、1 行に対して水平方向に整列しているはずです。

### フッターエクスペリエンスフラグメントを更新

1. フッターをレンダリングするエクスペリエンスフラグメントを [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) で開きます。
1. フラグメントのルート&#x200B;**コンテナ**&#x200B;を設定します。これが一番外側の **コンテナ**&#x200B;です。
   * **レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定します
1. **WKND ライトロゴ**&#x200B;を&#x200B;**コンテナ**&#x200B;の上部に画像として追加します。ロゴは前の手順でインストールしたパッケージに含まれています。
   * **WKND ライトロゴ**&#x200B;のレイアウトを変更して **2** 列幅にします。ハンドルを右から左にドラッグします。
   * ロゴの&#x200B;**代替テキスト**&#x200B;を「WKND ロゴライト」に設定します。
   * ロゴを `/content/wknd/us/en` ホームページに&#x200B;**リンク**&#x200B;するように設定します。
1. ロゴの下に&#x200B;**ナビゲーション**&#x200B;コンポーネントを追加します。**ナビゲーション**&#x200B;コンポーネントを設定します。
   * **ルートレベルを除外**&#x200B;を **1** に設定します。
   * 「**すべての子ページを収集**」のチェックをオフにします。
   * **ナビゲーション構造の深さ**&#x200B;を **1** に設定します。
   * **ナビゲーション**&#x200B;コンポーネントのレイアウトを変更して、**8** 列幅にします。ハンドルを右から左にドラッグします。

## 記事ページの作成

次に、記事ページテンプレートを使用してページを作成します。サイトのモックアップと一致するようにページのコンテンツを作成します。以下のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上記のビデオの大まかな手順：

1. Sites コンソール（[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)）に移動します。
1. **WKND**／**US**／**EN**／**Magazine** の下にページを作成します。
   * **記事ページ**&#x200B;テンプレートを選択します。
   * **プロパティ**&#x200B;で、**タイトル**&#x200B;を「Ultimate Guide to LA Skateparks」に設定します。
   * **名前**&#x200B;を「guide-la-skateparks」に設定します。
1. **By Author** タイトルを「By Stacey Roswells」というテキストで置き換えます。
1. **テキスト**&#x200B;コンポーネントを更新して、記事に入力する段落を含めます。コピーとして、テキストファイル [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt) を使用できます。
1. 別の&#x200B;**テキスト**&#x200B;コンポーネントを追加します。
   * コンポーネントを更新して、「There is no better place to shred than Los Angeles.」という引用を含めます。
   * フルスクリーンモードでリッチテキストエディターを編集し、上記の引用を変更して&#x200B;**引用ブロック**&#x200B;要素を使用します。
1. 続けて記事の本文を入力し、モックアップに一致させます。
1. **ダウンロード**&#x200B;コンポーネントを設定して、記事の PDF 版を使用できるようにします。
   * **ダウンロード**／**プロパティ**&#x200B;で、チェックボックスをクリックして **DAM アセットからタイトルを取得します**。
   * **説明**&#x200B;を「全文を読む」に設定します。
   * **アクションテキスト**&#x200B;を「Download PDF」に設定します。
1. **リスト**&#x200B;コンポーネントを設定します。
   * **リスト設定**／**次を使用してリストを作成**&#x200B;で、**子ページ**&#x200B;を選択します。
   * **親ページ**&#x200B;を `/content/wknd/us/en/magazine` に設定します。
   * **項目設定**&#x200B;で、「**項目をリンク**」および「**日付を表示**」にチェックを入れます。

## ノード構造の検査 {#node-structure}

この時点で、記事ページは明らかにスタイルが設定されていません。しかし、基本的な構造は整っています。次に、記事ページのノード構造を検査して、テンプレート、ページおよびコンポーネントの役割をさらに深く理解します。

ローカル AEM インスタンスで CRXDE-Lite ツールを使用して、基礎となるノード構造を表示します。

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) を開き、ツリーナビゲーションを使用して `/content/wknd/us/en/magazine/guide-la-skateparks` に移動します。

1. `la-skateparks` ページの下にある `jcr:content` ノードをクリックして、プロパティを表示します。

   ![JCR コンテンツのプロパティ](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   `cq:template` の値に注目してください。これは、以前に作成した記事ページテンプレートの `/conf/wknd/settings/wcm/templates/article-page` を指しています。

   また、`sling:resourceType` の値にも注目してください。これは、`wknd/components/page` を指しています。これは、AEM プロジェクトアーキタイプによって作成されたページコンポーネントであり、テンプレートに基づいてページをレンダリングします。

1. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` の下にある `jcr:content` ノードを展開し、ノード階層を表示します。

   ![JCR コンテンツの LA スケートパーク](assets/pages-templates/page-jcr-structure.png)

   作成したコンポーネントに各ノードを大まかにマップできます。プレフィックスに `container` が付いたノードを検査して、使用されている様々なレイアウトコンテナを識別できるかどうかを確認します。

1. 次に、`/apps/wknd/components/page` でページコンポーネントを検査します。CRXDE Lite でコンポーネントのプロパティを表示します。

   ![ページコンポーネントのプロパティ](assets/pages-templates/page-component-properties.png)

   ページコンポーネントの下には、`customfooterlibs.html` と `customheaderlibs.html` の 2 つの HTL スクリプトしかありません。*それでは、このコンポーネントはページをどのようにレンダリングするのでしょうか。*

   `sling:resourceSuperType` プロパティは、`core/wcm/components/page/v2/page` を指しています。このプロパティにより、WKND のページコンポーネントはコアコンポーネントページコンポーネントの&#x200B;**すべて**&#x200B;の機能を継承できます。これは、[プロキシコンポーネントパターン](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=ja#ProxyComponentPattern)と呼ばれるものの最初の例です。詳しくは、[こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=ja)を参照してください。

1. WKND コンポーネント内の別のコンポーネントである `/apps/wknd/components/breadcrumb` の `Breadcrumb` コンポーネントを検査します。同じ `sling:resourceSuperType` プロパティが見つかりますが、今回は `core/wcm/components/breadcrumb/v2/breadcrumb` を指していることに注目してください。これは、プロキシコンポーネントパターンを使用してコアコンポーネントを含める別の例です。実際、WKND コードベースのすべてのコンポーネントは、AEM コアコンポーネントのプロキシです（カスタムデモの HelloWorld コンポーネントを除く）。カスタムコードを記述する&#x200B;*前*&#x200B;に、コアコンポーネントの機能をできるだけ再利用することをお勧めします。

1. 次に、CRXDE Lite を使用して、`/libs/core/wcm/components/page/v2/page` にあるコアコンポーネントページを検査します。

   >[!NOTE]
   >
   > AEM 6.5 および 6.4 では、コアコンポーネントは `/apps/core/wcm/components` の下にあります。AEM as a Cloud Service では、コアコンポーネントは `/libs` の下にあり、自動的に更新されます。

   ![コアコンポーネントページ](assets/pages-templates/core-page-component-properties.png)

   このページの下には、多くのスクリプトファイルが含まれています。コアコンポーネントページには、多数の機能が含まれています。この機能は、メンテナンスや読みやすさを容易にするために、複数のスクリプトに分割されています。`page.html` を開いて `data-sly-include` を探すことで、HTL スクリプトが含まれているかどうかを追跡できます。

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   HTL を複数のスクリプトに分割するもう 1 つの理由は、プロキシコンポーネントが個々のスクリプトを上書きしてカスタムビジネスロジックを実装できるようにするためです。HTL スクリプトの `customfooterlibs.html` および `customheaderlibs.html` は、プロジェクトを実装することによって明示的に上書きされるように作成されています。

   [この記事を読むと、コンテンツページ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=ja)のレンダリングに編集可能なテンプレートが影響する方法について詳しく理解できます。

1. `/libs/core/wcm/components/breadcrumb/v2/breadcrumb` にあるパンくずリストなど、別のコアコンポーネントを検査します。パンくずコンポーネントのマークアップが最終的にどのように生成されるかを理解するには、`breadcrumb.html` スクリプトを表示します。

## ソース管理への設定の保存 {#configuration-persistence}

多くの場合、特に AEM プロジェクトの開始時には、テンプレートや関連するコンテンツポリシーなどの設定をソース管理に保持することが重要です。これにより、すべての開発者が同じセットのコンテンツと設定に対して作業することが保証され、環境間の一貫性がさらに確保できます。プロジェクトが一定の成熟度に達すると、テンプレートの管理作業を特別なパワーユーザーグループに引き継ぐことができます。


現時点では、テンプレートは他のコードと同様に扱われ、**記事ページテンプレート**をプロジェクトの一部として同期します。
これまで、コードは AEM プロジェクトから AEM のローカルインスタンスにプッシュされていました。**記事ページテンプレート**&#x200B;が AEM のローカルインスタンスで直接作成されたので、テンプレートを AEM プロジェクトに&#x200B;**読み込む**&#x200B;必要があります。**ui.content** モジュールは、この特定の目的のために AEM プロジェクトに含まれています。

次の一部の手順は、[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) プラグインを使用して VSCode IDE で実行されます。ただし、**読み込む**&#x200B;ように設定した IDE を使用したり、AEM のローカルインスタンスからコンテンツを読み込んだりする可能性があります。

1. VSCode で `aem-guides-wknd` プロジェクトを開きます。

1. プロジェクトエクスプローラーで、**ui.content** モジュールを展開します。`src` フォルダーを展開して、`/conf/wknd/settings/wcm/templates` に移動します。

1. `templates` フォルダーを[!UICONTROL 右クリック]し、**AEM サーバーから読み込む**&#x200B;を選択します。

   ![VSCode インポートテンプレート](assets/pages-templates/vscode-import-templates.png)

   `article-page` を読み込んで、`page-content` テンプレートおよび `xf-web-variation` テンプレートも更新する必要があります。

   ![更新されたテンプレート](assets/pages-templates/updated-templates.png)

1. コンテンツを読み込む手順を繰り返しますが、`/conf/wknd/settings/wcm/policies` から&#x200B;**ポリシー**&#x200B;フォルダーを選択します。

   ![VSCode 読み込みポリシー](assets/pages-templates/policies-article-page-template.png)

1. `ui.content/src/main/content/META-INF/vault/filter.xml` から `filter.xml` ファイルを検査します。

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

   `filter.xml` ファイルは、パッケージと共にインストールされるノードのパスを識別する役割を果たします。各フィルターの `mode="merge"` に注目してください。これは、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示しています。コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントでは、コンテンツを上書き&#x200B;**しません**。フィルター要素の操作の詳細については、[FileVault ドキュメント](https://jackrabbit.apache.org/filevault/filter.html)を参照してください。

   `ui.content/src/main/content/META-INF/vault/filter.xml` と `ui.apps/src/main/content/META-INF/vault/filter.xml` を比較して、各モジュールによって管理される様々なノードを理解します。

   >[!WARNING]
   >
   > WKND リファレンスサイトの一貫したデプロイメントを確実に行うために、プロジェクトの一部のブランチは、`ui.content` が JCR の変更を上書きするように設定されています。これは設計によるものです。つまり、ソリューションブランチ用にコードおよびスタイルが特定のポリシー用に記述されます。

## これで完了です。 {#congratulations}

Adobe Experience Manager Sites でテンプレートとページが作成されました。

### 次の手順 {#next-steps}

この時点で、記事ページは明らかにスタイルが設定されていません。[クライアントサイドライブラリとフロントエンドワークフロー](client-side-libraries.md)チュートリアルに従って、CSS と JavaScript を組み込み、グローバルスタイルをサイトに適用し、専用のフロントエンドビルドを統合するためのベストプラクティスを学びます。

[GitHub](https://github.com/adobe/aem-guides-wknd) で完成したコードを表示するか、コードを確認して Git ブランチ `tutorial/pages-templates-solution` でローカルにデプロイします。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) リポジトリのクローンを作成します。
1. `tutorial/pages-templates-solution` ブランチをチェックアウトします。
