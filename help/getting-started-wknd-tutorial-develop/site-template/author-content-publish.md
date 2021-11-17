---
title: オーサリングと公開の概要 | AEMクイックサイト作成
description: AEMのAdobe Experience Managerのページエディターを使用して、Web サイトのコンテンツを更新します。 オーサリングを容易にするためにコンポーネントを使用する方法について説明します。 AEM オーサー環境とパブリッシュ環境の違いを理解し、ライブサイトに変更を公開する方法を学びます。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 3%

---

# オーサリングと公開の概要 {#author-content-publish}

>[!CAUTION]
>
> 現在、クイックサイト作成ツールはテクニカルプレビューです。 テストおよび評価の目的で使用できるようになり、Adobeサポートに同意しない限り、実稼動での使用は意図されません。

ユーザーが Web サイトのコンテンツを更新する方法を理解することが重要です。 この章では、以下のような **コンテンツ作成者** 前の章で生成したサイトを編集用に更新します。 チャプターの最後に変更を公開して、ライブサイトの更新方法を理解します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [サイトの作成](./create-site.md) チャプターが完了しました。

## 目的 {#objective}

1. の概念を理解する **ページ** および **コンポーネント** AEM Sites
1. Web サイトのコンテンツを更新する方法を説明します。
1. ライブサイトに変更を公開する方法を説明します。

## 新しいページを作成 {#create-page}

通常、Web サイトは複数ページに分割されて、複数ページでのエクスペリエンスを形成します。 AEMはコンテンツを同じ方法で構造化します。 次に、サイトの新しいページを作成します。

1. AEMにログインします。 **作成者** 前の章で使用したサービス。
1. 「AEM Start」画面で、 **サイト** > **WKND サイト** > **英語** > **記事**
1. 右上隅で、 **作成** > **ページ**.

   ![ページを作成](assets/author-content-publish/create-page-button.png)

   これで、 **ページを作成** ウィザード。

1. を選択します。 **記事ページ** テンプレートを選択し、 **次へ**.

   AEMのページは、ページテンプレートに基づいて作成されます。 ページテンプレートについて詳しくは、 [ページテンプレート](page-templates.md) チャプター。

1. の下 **プロパティ** 入力 **タイトル** &quot;Hello World&quot;の
1. を **名前** 次の `hello-world` をクリックし、 **作成**.

   ![最初のページのプロパティ](assets/author-content-publish/initial-page-properties.png)

1. ダイアログのポップアップで、 **開く** をクリックして、新しく作成されたページを開きます。

## コンポーネントのオーサリング {#author-component}

AEMコンポーネントは、Web ページの小さなモジュラー構築ブロックと考えることができます。 UI を論理チャンクまたはコンポーネントに分割することで、管理がはるかに容易になります。 コンポーネントを再利用するには、コンポーネントを設定できる必要があります。 これは、オーサーダイアログを通じて実行します。

AEMは、 [コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja) 実稼動環境で使用する準備が整っています。 この **コアコンポーネント** ～のような基本的な要素の範囲 [テキスト](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=ja) および [画像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=ja) を使用して、 [カルーセル](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=ja).

次に、AEMページエディターを使用して、いくつかのコンポーネントを作成します。

1. 次に移動： **Hello World** 前の演習で作成したページ。
1. 次の場所にいることを確認します。 **編集** モードに切り替えて、左側のサイドレールで **コンポーネント** アイコン

   ![ページエディターのサイドレール](assets/author-content-publish/page-editor-siderail.png)

   これにより、コンポーネントライブラリが開き、ページで使用できる使用可能なコンポーネントが一覧表示されます。

1. 下にスクロールして **ドラッグ&amp;ドロップ** a **テキスト (v2)** ページのメインの編集可能領域上のコンポーネント。

   ![テキストコンポーネントをドラッグ&amp;ドロップ](assets/author-content-publish/drag-drop-text-cmp.png)

1. 次をクリック： **テキスト** ハイライト表示するコンポーネントを選択し、 **レンチ** アイコン ![レンチアイコン](assets/author-content-publish/wrench-icon.png) をクリックして、コンポーネントのダイアログを開きます。 テキストを入力し、変更をダイアログに保存します。

   ![リッチテキストコンポーネント](assets/author-content-publish/rich-text-populated-component.png)

   この **テキスト** コンポーネントは、ページ上にリッチテキストを表示する必要があります。

1. 上記の手順を繰り返します。ただし、 **画像 (v2)** コンポーネントをページに追加します。 を開きます。 **画像** コンポーネントのダイアログ。

1. 左側のレールで、 **アセットファインダー** クリックして **Assets** アイコン ![アセットアイコン](assets/author-content-publish/asset-icon.png).
1. **ドラッグ&amp;ドロップ** 画像をコンポーネントのダイアログに追加し、 **完了** 変更を保存します。

   ![アセットをダイアログに追加](assets/author-content-publish/add-asset-dialog.png)

1. ページ上に、 **タイトル**, **ナビゲーション**, **検索** それは修正されました。 これらの領域は、ページテンプレートの一部として設定され、個々のページでは変更できません。 これについては、次の章で詳しく説明します。

他のコンポーネントを自由に試してみてください。 各 [コアコンポーネントは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). に関する詳細なビデオシリーズ [ページのオーサリングはこちらから参照できます](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 更新を公開 {#publish-updates}

AEM環境は **オーサーサービス** および **パブリッシュサービス**. この章では、 **オーサーサービス**. サイト訪問者が変更を表示するには、変更を **パブリッシュサービス**.

![概要図](assets/author-content-publish/author-publish-high-level-flow.png)

*オーサーからパブリッシュへのコンテンツの大まかなフロー*

**1.** コンテンツ作成者がサイトのコンテンツを更新します。 更新内容のプレビュー、確認、承認を行って、ライブにプッシュすることができます。

**2.**&#x200B;コンテンツが公開されました。パブリッシュは、オンデマンドで実行することも、将来の日付に対してスケジュール設定することもできます。

**3.** サイトの訪問者には、変更がパブリッシュサービスに反映されているのが表示されます。

### 変更を公開

次に、変更を公開します。

1. AEM Start 画面からに移動します。 **サイト** をクリックし、 **WKND サイト**.
1. 次をクリック： **公開を管理** をクリックします。

   ![公開を管理](assets/author-content-publish/click-manage-publiciation.png)

   これはまったく新しいサイトなので、すべてのページを公開し、公開を管理ウィザードを使用して、公開する必要があるものを正確に定義できます。

1. の下 **オプション** デフォルト設定を次のままにします。 **公開** そしてそれをスケジュールする **今すぐ**. 「**次へ**」をクリックします。
1. の下 **範囲**&#x200B;を選択し、 **WKND サイト** をクリックし、 **子設定を含める**. ダイアログで、 **子を含める**. 残りのボックスのチェックをオフにして、サイト全体が公開されるようにします。

   ![公開範囲を更新](assets/author-content-publish/update-scope-publish.png)

1. 次をクリック： **公開済みの参照** 」ボタンをクリックします。 ダイアログで、すべてがオンになっていることを確認します。 これには、 **標準サイトテンプレート** およびサイトテンプレートによって生成されるいくつかの設定。 クリック **完了** を更新します。

   ![参照を公開](assets/author-content-publish/publish-references.png)

1. 最後に、の横にあるチェックボックスをオンにします。 **WKND サイト** をクリックし、 **次へ** をクリックします。
1. 内 **ワークフロー** ステップ、 **ワークフロータイトル**. 任意のテキストを指定でき、後で監査証跡の一部として使用する場合に役立ちます。 「初期公開」と入力し、 **公開**.

![ワークフローステップの初期公開](assets/author-content-publish/workflow-step-publish.png)

## 公開済みコンテンツを表示 {#publish}

次に、パブリッシュサービスに移動して、変更を表示します。

1. パブリッシュサービスの URL を簡単に取得するには、オーサー URL をコピーして、 `author` 次の単語 `publish`. 次に例を示します。

   * **作成者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **公開 URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 追加 `/content/wknd.html` を公開 URL に追加して、最終的な URL が次のようになるようにします。 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 変更 `wknd.html` サイトの名前に一致させる ( [サイトの作成](create-site.md).

1. AEMオーサリング機能を使用せずに、サイトが表示される公開 URL に移動します。

   ![公開済みサイト](assets/author-content-publish/publish-url-update.png)

1. の使用 **ナビゲーション** メニュークリック **記事** > **Hello World** をクリックして、前に作成した Hello World ページに移動します。
1. に戻る **AEM オーサーサービス** ページエディターで、コンテンツをさらに変更します。
1. これらの変更を、ページエディター内で **ページプロパティ** アイコン/ **ページを公開**

   ![直接公開](assets/author-content-publish/page-editor-publish.png)

1. に戻る **AEM パブリッシュサービス** をクリックして変更を表示します。 おそらく **not** すぐに更新を確認します。 これは、 **AEM パブリッシュサービス** 次を含む [Apache Web サーバーおよび CDN を介したキャッシュ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). デフォルトでは、HTMLコンテンツは約 5 分間キャッシュされます。

1. テストやデバッグの目的でキャッシュをバイパスするには、次のようなクエリパラメーターを追加します。 `?nocache=true`. URL は次のようになります。 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 使用可能なキャッシュ方法と設定の詳細 [ここにあります](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. また、Cloud Manager でパブリッシュサービスへの URL を見つけることもできます。 次に移動： **Cloud Manager プログラム** > **環境** > **環境**.

   ![パブリッシュサービスを表示](assets/author-content-publish/view-environment-segments.png)

   の下 **環境セグメント** 次のリンクを検索： **作成者** および **公開** サービス。

## おめでとうございます。 {#congratulations}

これで、AEMサイトに対する変更を作成して公開しました。

### 次の手順 {#next-steps}

実際の実装計画では、モックアップと UI デザインを含むサイトは通常、サイトの作成よりも前に作成されます。 Adobe XD UI キットを使用して、でAdobe Experience Manager Sitesの実装を設計および高速化する方法を説明します。 [Adobe XDでの UI 計画](./ui-planning-adobe-xd.md).

AEM Sitesの機能を引き続き参照しますか？ ～に関する章にすぐに飛び込むのは自由だ [ページテンプレート](./page-templates.md) を使用して、ページテンプレートとページの関係を理解できます。


