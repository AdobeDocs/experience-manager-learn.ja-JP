---
title: AEM Sitesの使い始めに — ページとテンプレート
seo-title: AEM Sitesの使い始めに — ページとテンプレート
description: 基本ページコンポーネントと編集可能なテンプレートとの関係について説明します。 コアコンポーネントがプロジェクト内でどのようにプロキシ化されるかを理解し、Adobe XDのモックアップに基づいて、適切に構成された記事ページテンプレートを作成するための、編集可能なテンプレートの高度なポリシー設定を学びます。
sub-product: サイト
feature: コアコンポーネント、編集可能なテンプレート
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
topic: コンテンツ管理、開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: b5b43ae8231bf23e0c53777b1e9c16bcc3fc188a
workflow-type: tm+mt
source-wordcount: '3104'
ht-degree: 2%

---


# ページとテンプレート{#pages-and-template}

この章では、基本ページコンポーネントと編集可能なテンプレートとの関係について説明します。 [AdobeXD](https://www.adobe.com/products/xd.html)のモックアップに基づいて、スタイル設定されていない記事テンプレートを作成します。 テンプレートの構築の過程で、コアコンポーネントと編集可能なテンプレートの高度なポリシー設定について説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルが構築する基本行コードを調べます。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の`tutorial/pages-templates-start`ブランチを調べます。

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)に常に表示できます。また、ブランチ`tutorial/pages-templates-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 目的

1. Inspectは、Adobe XDで作成されたページデザインで、コアコンポーネントにマッピングします。
1. 編集可能なテンプレートの詳細と、ページコンテンツの詳細な制御を強制するためにポリシーを使用する方法を理解します。
1. テンプレートとページのリンク方法

## 作成する内容 {#what-you-will-build}

チュートリアルのこの部分では、新しい記事ページの作成と共通の構造との整合に使用できる新しい記事ページテンプレートを作成します。 記事ページテンプレートは、AdobeXDで作成されたデザインとUIキットに基づいて作成されます。 この章では、テンプレートの構造またはスケルトンの作成にのみ焦点を当てます。 スタイルは実装されませんが、テンプレートとページは機能します。

![記事のページデザインとスタイル設定解除バージョン](assets/pages-templates/what-you-will-build.png)

## Adobe XDとのUIの計画{#adobexd}

ほとんどの場合、モックアップと静的デザインを含む新しいWebサイト開始の作成を計画します。 [Adobe](https://www.adobe.com/products/xd.html) XDは、ユーザーエクスペリエンスを構築するデザインツールです。次に、UIキットとモックアップを調べ、記事ページテンプレートの構造の計画に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**WKND記事デザインファイル [をダウンロードします](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 汎用の[AEMコアコンポーネントUIキットも、カスタムプロジェクトの起点として](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)提供されています。

## 記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これは新しいページを作成するための基本として使用されます。テンプレートは、結果のページ、初期コンテンツ、許可されるコンポーネントの構造を定義します。

[編集可能なテンプレート](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)には、主に3つの領域があります。

1. **構造**  — テンプレートの一部であるコンポーネントを定義します。コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ**  — テンプレートを開始するコンポーネントを定義します。コンテンツ作成者はこれらを編集または削除できます。
1. **ポリシー**  — コンポーネントの動作方法と作成者が使用できるオプションに関する設定を定義します。

次に、モックアップの構造と一致するAEMで新しいテンプレートを作成します。 これは、AEMのローカルインスタンスで発生します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

以下のビデオの高レベルの手順を示します。

### 構造の設定

1. **ページテンプレートタイプ**&#x200B;を使用して、**記事ページ**&#x200B;という名前の新しいテンプレートを作成します。
1. **構造**&#x200B;モードに切り替えます。
1. テ追加ンプレートの上部の&#x200B;**ヘッダー**&#x200B;として機能する&#x200B;**エクスペリエンスフラグメント**&#x200B;コンポーネント。
   * `/content/experience-fragments/wknd/us/en/site/header/master`を指すようにコンポーネントを設定します。
   * ポリシーを「**ページヘッダー**」に設定し、**デフォルトの要素**&#x200B;が`header`に設定されていることを確認します。 `header`要素は、次の章でCSSを使用してターゲットにします。
1. テンプレートの追加下部で&#x200B;**フッター**&#x200B;として機能する&#x200B;**エクスペリエンスフラグメント**&#x200B;コンポーネント。
   * `/content/experience-fragments/wknd/us/en/site/footer/master`を指すようにコンポーネントを設定します。
   * ポリシーを「**ページフッター**」に設定し、**デフォルトの要素**&#x200B;が`footer`に設定されていることを確認します。 `footer`要素は、次の章でCSSを使用してターゲットにします。
1. テンプレートの最初の作成時に含めた&#x200B;**main**&#x200B;コンテナをロックします。
   * ポリシーを&#x200B;**Page Main**&#x200B;に設定し、**デフォルトの要素**&#x200B;が`main`に設定されていることを確認します。 `main`要素は、次の章でCSSを使用してターゲットにします。
1. 追加&#x200B;**メイン**&#x200B;コンテナの&#x200B;**画像**&#x200B;コンポーネント。
   * **画像**&#x200B;コンポーネントのロックを解除します。
1. メ追加インコンテナの&#x200B;**Image**&#x200B;コンポーネントの下の&#x200B;**階層リンク**&#x200B;コンポーネント。
   * **記事ページ — パンくず**&#x200B;という名前の&#x200B;**パンくず**&#x200B;コンポーネント用に新しいポリシーを作成します。 **ナビゲーション開始レベル**&#x200B;を&#x200B;**4**&#x200B;に設定します。
1. 追加&#x200B;**階層リンク**&#x200B;コンポーネントの下、および&#x200B;**main**&#x200B;コンテナ内の&#x200B;**コンテナ**&#x200B;コンポーネント。 これは、テンプレートの&#x200B;**コンテンツコンテナ**&#x200B;として機能します。
   * **コンテンツ**&#x200B;コンテナーのロックを解除します。
   * ポリシーを&#x200B;**ページコンテンツ**&#x200B;に設定します。
1. 追加&#x200B;**コンテンツコンテナ**&#x200B;の下にある別の&#x200B;**コンテナ**&#x200B;コンポーネント。 これは、テンプレートの&#x200B;**サイドレール**&#x200B;コンテナとして機能します。
   * **サイドレール**&#x200B;コンテナのロックを解除します。
   * 「**記事ページ — サイドレール**」という名前の新しいポリシーを作成します。
   * **WKND Sites Project - Content**&#x200B;の下の&#x200B;**許可されているコンポーネント**&#x200B;を、次を含むように設定します。**ボタン**、**ダウンロード**、**リスト**、**画像**、**セパレータ**、**ソーシャルメディア共有**、**テキスト**&#x200B;と&#x200B;**タイトル**。
1. ページルートコンテナのポリシーを更新します。 これは、テンプレートの最も外側にあるコンテナです。 ポリシーを&#x200B;**ページルート**&#x200B;に設定します。
   * **コンテナ設定**&#x200B;で、**レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定します。
1. **コンテンツコンテナ**&#x200B;のレイアウトモードを有効にします。 ハンドルを右から左にドラッグし、コンテナを8列幅に縮小します。
1. **サイドレールコンテナ**&#x200B;のレイアウトモードを有効にします。 ハンドルを右から左にドラッグし、コンテナを4列幅に縮小します。 次に、左ハンドルを左から右へ1列にドラッグして、コンテナ3列を幅にし、**コンテンツコンテナ**&#x200B;の間に1列のギャップを残します。
1. モバイルエミュレーターを開き、モバイルブレークポイントに切り替えます。 レイアウトモードを再度有効にし、**コンテンツコンテナ**&#x200B;と&#x200B;**サイドレールコンテナ**&#x200B;をページの全幅にします。 これにより、モバイルブレークポイント内でコンテナが垂直に積み重ねられます。
1. **コンテンツコンテナ**&#x200B;の&#x200B;**テキスト**&#x200B;コンポーネントのポリシーを更新します。
   * ポリシーを&#x200B;**コンテンツテキスト**&#x200B;に設定します。
   * **プラグイン**/**段落スタイル**&#x200B;の下で、**段落スタイルを有効にする**&#x200B;をオンにし、**引用ブロック**&#x200B;が有効になっていることを確認します。

### 初期コンテンツ設定

1. **初期コンテンツ**&#x200B;モードに切り替えます。
1. 追加&#x200B;**コンテンツコンテナ**&#x200B;の&#x200B;**タイトル**&#x200B;コンポーネント。 これは記事のタイトルとして機能します。 空白のままにすると、現在のページのタイトルが自動的に表示されます。
1. 最初の追加Titleコンポーネントの下の2番目の&#x200B;**Title**&#x200B;コンポーネント。
   * コンポーネントを次のテキストで設定します。「作成者」。 これはテキストプレースホルダーです。
   * タイプを`H4`に設定します。
1. 追加&#x200B;**作成者**&#x200B;のタイトルコンポーネントの下にあるテキスト&#x200B;**コンポーネント。**
1. 追加&#x200B;**サイドレールコンテナ**&#x200B;の&#x200B;**タイトル**&#x200B;コンポーネント。
   * コンポーネントを次のテキストで設定します。「このストーリーを共有」を参照してください。
   * タイプを`H5`に設定します。
1. 追加&#x200B;**ソーシャルメディア共有**&#x200B;コンポーネント（**このストーリーを共有**&#x200B;タイトルコンポーネントの下）。
1. 追加&#x200B;**ソーシャルメディア共有**&#x200B;コンポーネントの下にある&#x200B;**区切り記号**&#x200B;コンポーネント。
1. 追加&#x200B;****&#x200B;セパレータ&#x200B;**コンポーネントの下に**&#x200B;コンポーネントをダウンロードします。
1. 追加&#x200B;****&#x200B;コンポーネントの下にある&#x200B;**リスト**&#x200B;コンポーネント。
1. テンプレートの&#x200B;**初期ページプロパティ**&#x200B;を更新します。
   * **ソーシャルメディア**/**ソーシャルメディアシェア**&#x200B;の下で、**Facebook**&#x200B;と&#x200B;**Pinterest**&#x200B;をチェックします

### テンプレートを有効にし、サムネールを追加する

1. テンプレートコンソールでテンプレートを表示するには、[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)に移動します。
1. **記事ページのテンプレートを** 有効にします。
1. 記事ページテンプレートのプロパティを編集し、次のサムネールをアップロードして、記事ページテンプレートを使用して作成されたページをすばやく特定します。

   ![記事ページテンプレートのサムネール](assets/pages-templates/article-page-template-thumbnail.png)

## ヘッダーとフッターをエクスペリエンスフラグメントに更新{#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する場合、一般に、[エクスペリエンスフラグメント](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)を使用します。 エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1つの参照可能なコンポーネントを作成できます。 エクスペリエンスフラグメントには、マルチサイト管理と[ローカライゼーション](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)をサポートする利点があります。

AEMプロジェクトのアーキタイプで、ヘッダーとフッターが生成されました。 次に、エクスペリエンスフラグメントを更新して、モックアップと一致させます。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

以下のビデオの高レベルの手順を示します。

1. サンプルコンテンツパッケージ&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**&#x200B;をダウンロードします。
1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)にあるPackage Managerを使用して、コンテンツパッケージをアップロードしてインストールします。
1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)にあるエクスペリエンスフラグメントに使用されるテンプレートである、Webバリエーションテンプレートを更新します。
   * テンプレートの&#x200B;**コンテナ**&#x200B;コンポーネントのポリシーを更新します。
   * ポリシーを&#x200B;**XFルート**&#x200B;に設定します。
   * 「**許可されているコンポーネント**」で、コンポーネントグループ&#x200B;**WKNDサイトプロジェクト — 構造**&#x200B;を選択し、**言語ナビゲーション**、**ナビゲーション**、**クイック検索**&#x200B;コンポーネントを含めます。

### ヘッダーエクスペリエンスフラグメントを更新

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)にあるヘッダーをレンダリングするエクスペリエンスフラグメントを開きます。
1. フラグメントのルート&#x200B;**コンテナ**&#x200B;を設定します。 これは、**コンテナ**&#x200B;の最外側です。
   * **レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定
1. 追加&#x200B;**WKND Dark Logo**&#x200B;を画像として&#x200B;**コンテナ**&#x200B;の上に配置します。 前の手順でインストールしたパッケージにロゴが含まれています。
   * **WKNDダークロゴ**&#x200B;のレイアウトを&#x200B;**2**&#x200B;列幅に変更します。 ハンドルを右から左にドラッグします。
   * 「WKND Logo」の&#x200B;**代替テキスト**&#x200B;を使用してロゴを設定します。
   * ロゴを&#x200B;**ホームページを**&#x200B;リンク`/content/wknd/us/en`に設定します。
1. 既にページに配置されている&#x200B;**Navigation**&#x200B;コンポーネントを設定します。
   * **Exclude Root Levels**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * **ナビゲーション構造の深さ**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * **ナビゲーション**&#x200B;コンポーネントのレイアウトを&#x200B;**8**&#x200B;列幅に変更します。 ハンドルを右から左にドラッグします。
1. **言語ナビゲーション**&#x200B;コンポーネントを削除します。
1. **検索**&#x200B;コンポーネントのレイアウトを変更して、**2**&#x200B;列幅にします。 ハンドルを右から左にドラッグします。 これで、すべてのコンポーネントが1行に水平に並べられます。

### フッターエクスペリエンスフラグメントを更新

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)にあるフッターをレンダリングするエクスペリエンスフラグメントを開きます。
1. フラグメントのルート&#x200B;**コンテナ**&#x200B;を設定します。 これは、**コンテナ**&#x200B;の最外側です。
   * **レイアウト**&#x200B;を&#x200B;**レスポンシブグリッド**&#x200B;に設定
1. 追加&#x200B;**WKND Light Logo**&#x200B;を画像として&#x200B;**コンテナ**&#x200B;の上に配置します。 前の手順でインストールしたパッケージにロゴが含まれています。
   * **WKND Light Logo**&#x200B;のレイアウトを&#x200B;**2**&#x200B;列幅に変更します。 ハンドルを右から左にドラッグします。
   * 「WKND Logo Light」の&#x200B;**代替テキスト**&#x200B;を使用してロゴを設定します。
   * ロゴを&#x200B;**ホームページを**&#x200B;リンク`/content/wknd/us/en`に設定します。
1. ロゴの追加下にある&#x200B;**ナビゲーション**&#x200B;コンポーネント。 **ナビゲーション**&#x200B;コンポーネントを構成します。
   * **Exclude Root Levels**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * 「**すべての子ページを収集**」のチェックを外します。
   * **ナビゲーション構造の深さ**&#x200B;を&#x200B;**1**&#x200B;に設定します。
   * **ナビゲーション**&#x200B;コンポーネントのレイアウトを&#x200B;**8**&#x200B;列幅に変更します。 ハンドルを右から左にドラッグします。

## 記事ページの作成

次に、記事ページテンプレートを使用して新しいページを作成します。 ページのコンテンツを作成し、サイトのモックアップと一致させます。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

以下のビデオの高レベルの手順を示します。

1. [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)にあるサイトコンソールに移動します。
1. **WKND** > **US** > **EN** > **Magazine**&#x200B;の下に新しいページを作成します。
   * **記事ページ**&#x200B;テンプレートを選択します。
   * **プロパティ**&#x200B;の下で、**タイトル**&#x200B;を&quot;Ultimate Guide to LA Skateparks&quot;に設定します
   * **名前**&#x200B;を&quot;guide-la-skateparks&quot;に設定
1. 「**作成者**&#x200B;のタイトル」を「By Stacey Roswells」に置き換えます。
1. 記事に入力する段落を含めるように&#x200B;**テキスト**&#x200B;コンポーネントを更新します。 次のテキストファイルをコピーとして使用できます。[la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)に置き換えます。
1. 追加別の&#x200B;**テキスト**&#x200B;コンポーネント。
   * 見積もりを含めるようにコンポーネントを更新します。「ロサンゼルスは、もっといい場所がない」
   * フルスクリーンモードでリッチテキストエディタを編集し、上記の引用符を変更して&#x200B;**引用ブロック**&#x200B;要素を使用します。
1. 記事の本文の入力を続けて、モックアップに一致させます。
1. 記事のPDFバージョンを使用するように&#x200B;**Download**&#x200B;コンポーネントを設定します。
   * **ダウンロード**/**プロパティ**&#x200B;の下で、「**DAMアセット**&#x200B;からタイトルを取得」のチェックボックスをクリックします。
   * **説明**&#x200B;を次の値に設定します。「Get the Full Story」を参照してください。
   * **アクションテキスト**&#x200B;を次に設定します。「PDFをダウンロード」
1. **リスト**&#x200B;コンポーネントを構成します。
   * **リスト設定**/****&#x200B;を使用してリストを構築>の下で、「**子ページ**」を選択します。
   * **親ページ**&#x200B;を`/content/wknd/us/en/magazine`に設定します。
   * 「**アイテム設定**」で、「**アイテムをリンク**」をチェックし、「**日付を表示**」をチェックします。

## Inspectノード構造{#node-structure}

この時点で、記事のページのスタイルは明確に解除されます。 ただし、基本構造は定められている。 次に、記事ページのノード構造を調べ、テンプレート、ページ、コンポーネントの役割をより深く理解します。

ローカルAEMインスタンスでCRXDE-Liteツールを使用して、基になるノード構造を表示します。

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)を開き、ツリーナビゲーションを使用して`/content/wknd/us/en/magazine/guide-la-skateparks`に移動します。

1. `la-skateparks`ページの下の`jcr:content`ノードをクリックし、次のプロパティを表示します。

   ![JCRコンテンツプロパティ](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   `cq:template`の値（`/conf/wknd/settings/wcm/templates/article-page`、先ほど作成した記事ページテンプレート）に注目してください。

   また、`sling:resourceType`の値（`wknd/components/page`を指す）にも注意してください。 これは、AEMプロジェクトのアーキタイプによって作成されるページコンポーネントで、テンプレートに基づいてページをレンダリングします。

1. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`の下の`jcr:content`ノードを展開し、表示階層を示します。

   ![JCRコンテンツLAスケートパークス](assets/pages-templates/page-jcr-structure.png)

   各ノードを、オーサリングされたコンポーネントに緩やかにマッピングできるはずです。 `container`のプレフィックスが付いたノードを検査することで使用される様々なレイアウトコンテナを特定できるかどうかを確認してください。

1. 次に、`/apps/wknd/components/page`にあるページコンポーネントを調べます。 CRXDE Lite内のコンポーネントプロパティの表示:

   ![ページコンポーネントプロパティ](assets/pages-templates/page-component-properties.png)

   ページコンポーネントの下には、2つのHTLスクリプト（`customfooterlibs.html`と`customheaderlibs.html`）しかありません。 *では、このコンポーネントはどのようにページをレンダリングするのか？*

   `sling:resourceSuperType`プロパティは`core/wcm/components/page/v2/page`を指します。 このプロパティを使用すると、WKNDのページコンポーネントは、コアコンポーネントページコンポーネントの機能の&#x200B;**すべて**&#x200B;を継承できます。 これは、[プロキシコンポーネントパターン](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)と呼ばれるものの最初の例です。詳しくは、[こちら](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html)を参照してください。。

1. WKNDコンポーネント内の別のコンポーネント`Breadcrumb`をInspectが配置している場所：`/apps/wknd/components/breadcrumb`. 同じ`sling:resourceSuperType`プロパティが見つかりますが、今回は`core/wcm/components/breadcrumb/v2/breadcrumb`を指すことに注意してください。 これは、Proxyコンポーネントパターンを使用してCoreコンポーネントを含める別の例です。 実際、WKNDコードベースのコンポーネントはすべて、AEMコアコンポーネントのプロキシです（当社の有名なHelloWorldコンポーネントを除く）。 カスタムコードを記述する前に、コアコンポーネントの機能をできるだけ多く&#x200B;*使用し直すことをお勧めします。*

1. 次に、次のCRXDE Liteを使用して、`/libs/core/wcm/components/page/v2/page`のコアコンポーネントページを調べます。

   >[!NOTE]
   >
   > AEM 6.5/6.4では、コアコンポーネントは`/apps/core/wcm/components`の下にあります。 AEMでは、Cloud Serviceとしてコアコンポーネントは`/libs`の下に配置され、自動的に更新されます。

   ![コアコンポーネントページ](assets/pages-templates/core-page-component-properties.png)

   このページの下には、さらに多くのスクリプトが含まれています。 コアコンポーネントページには多くの機能が含まれています。 この機能は、メンテナンスと読みやすさを容易にするために、複数のスクリプトに分割されています。 `page.html`を開き、`data-sly-include`を探すことで、HTLスクリプトを含めるかどうかを追跡できます。

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

   HTLを複数のスクリプトに分割するもう1つの理由は、プロキシコンポーネントが、カスタムのビジネスロジックを実装する個々のスクリプトを上書きできるようにすることです。 HTLスクリプト`customfooterlibs.html`と`customheaderlibs.html`は、明示的な目的で作成され、プロジェクトの実装によって上書きされます。

   編集可能なテンプレートが[コンテンツページのレンダリングにどのように影響するかについては、この記事](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)を参照して詳しく知ることができます。

1. `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`の階層リンクのように、別のコアコンポーネントをInspectします。 `breadcrumb.html`スクリプトを表示して、階層リンクコンポーネントのマークアップが最終的にどのように生成されるかを理解します。

## ソース管理{#configuration-persistence}への構成の保存

多くの場合、特にAEMプロジェクトの最初に、テンプレートや関連するコンテンツポリシーなどの設定を保持してソース管理を行うことが重要です。 これにより、すべての開発者が同じコンテンツと設定のセットに対して作業を行い、環境間の一貫性を確保できます。 プロジェクトが一定の成熟度に達したら、テンプレートの管理方法をパワーユーザーの特別なグループに変えることができます。

ここでは、テンプレートを他のコードの部分と同様に扱い、プロジェクトの一部として&#x200B;**記事ページテンプレート**&#x200B;を下方向に同期します。 今までは、AEMプロジェクトからAEMのローカルインスタンスに&#x200B;**プッシュ**&#x200B;コードを持っていました。 **記事ページテンプレート**&#x200B;は、AEMのローカルインスタンスで直接作成されたものなので、AEMプロジェクトにテンプレートを&#x200B;**読み込む**&#x200B;必要があります。 この目的のため、**ui.content**&#x200B;モジュールはAEMプロジェクトに含まれています。

次のいくつかの手順は、[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)プラグインを使用してVSCode IDEを使用して行いますが、**import**&#x200B;に設定したIDEを使用したり、AEMのローカルインスタンスからコンテンツをインポートしたりできます。

1. VSCodeで`aem-guides-wknd`プロジェクトを開きます。

1. プロジェクトエクスプローラーで&#x200B;**ui.content**&#x200B;モジュールを展開します。 `src`フォルダーを展開し、`/conf/wknd/settings/wcm/templates`に移動します。

1. [!UICONTROL フォルダーを右] クリックし、「AEM Server `templates` から **読み込み**」を選択します。

   ![VSCodeインポートテンプレート](assets/pages-templates/vscode-import-templates.png)

   `article-page`を読み込み、`page-content`、`xf-web-variation`テンプレートも更新する必要があります。

   ![更新されたテンプレート](assets/pages-templates/updated-templates.png)

1. コンテンツを読み込む手順を繰り返しますが、`/conf/wknd/settings/wcm/policies`にある&#x200B;**ポリシー**&#x200B;フォルダーを選択します。

   ![VSCodeインポートポリシー](assets/pages-templates/policies-article-page-template.png)

1. `filter.xml`ファイルをInspect`ui.content/src/main/content/META-INF/vault/filter.xml`に置きます。

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

   `filter.xml`ファイルは、パッケージと共にインストールされるノードのパスを識別します。 各フィルターの`mode="merge"`は、既存のコンテンツは変更されず、新しいコンテンツのみが追加されることを示しています。 コンテンツ作成者がこれらのパスを更新する可能性があるので、コードのデプロイメントではコンテンツを&#x200B;**上書きしない**&#x200B;ことが重要です。 フィルタ要素の操作の詳細については、[FileVaultドキュメント](https://jackrabbit.apache.org/filevault/filter.html)を参照してください。

   `ui.content/src/main/content/META-INF/vault/filter.xml`と`ui.apps/src/main/content/META-INF/vault/filter.xml`を比較して、各モジュールで管理される異なるノードを理解します。

   >[!WARNING]
   >
   > WKNDリファレンスサイトで一貫したデプロイを行うために、プロジェクトの一部のブランチがセットアップされ、`ui.content`によってJCRの変更が上書きされます。 これは、特定のポリシーに対してコード/スタイルが書き込まれるので、ソリューションの分岐などに対して設計されています。

## これで完了です! {#congratulations}

新しいテンプレートとページがAdobe Experience Manager Sitesに作成されました。

### 次の手順 {#next-steps}

この時点で、記事のページのスタイルは明確に解除されます。 [クライアント側ライブラリとフロントエンドワークフロー](client-side-libraries.md)のチュートリアルに従って、CSSとJavascriptを含めて、サイトにグローバルスタイルを適用し、専用のフロントエンドビルドを統合するベストプラクティスを学びます。

[GitHub](https://github.com/adobe/aem-guides-wknd)上の完了したコードを表示するか、Gitブラック`tutorial/pages-templates-solution`上のローカルにコードを確認して展開します。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。
1. `tutorial/pages-templates-solution`ブランチを調べます。
