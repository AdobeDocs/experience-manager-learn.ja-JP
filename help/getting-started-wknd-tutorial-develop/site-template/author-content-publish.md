---
title: コンテンツの作成と変更の発行
seo-title: はじめに —AEM Sites — コンテンツの作成と変更の公開
description: Webサイトのコンテンツを更新するには、AEMのAdobe Experience Managerのページエディターを使用します。 オーサリングを容易にするためにコンポーネントを使用する方法を説明します。 AEM作成者と発行環境の違いを理解し、変更をライブサイトに発行する方法を学びます。
sub-product: サイト
version: Cloud Service
type: Tutorial
topic: コンテンツ管理
feature: コアコンポーネント，ページエディター
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 3%

---


# コンテンツの作成と変更の発行{#author-content-publish}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年下半期にリリースされます。 関連ドキュメントは、プレビュー目的で利用できます。

ユーザーがWebサイトのコンテンツを更新する方法を理解することが重要です。 この章では、**コンテンツ作成者**&#x200B;の人物を採用し、前の章で生成したサイトに対して編集上の更新を行います。 チャプターの最後に、変更を公開して、本番用サイトの更新方法を理解します。

## 前提条件 {#prerequisites}

これはマルチパート形式のチュートリアルで、[「サイトを作成する](./create-site.md)」の章で概要を説明した手順が完了していることを前提としています。

## 目的 {#objective}

1. AEM Sitesの&#x200B;**ページ**&#x200B;と&#x200B;**コンポーネント**&#x200B;の概念を理解します。
1. Webサイトのコンテンツを更新する方法を説明します。
1. 変更をライブサイトに発行する方法を学びます。

## 新しいページを作成{#create-page}

通常、Webサイトはページに分割され、複数ページでのエクスペリエンスを形成します。 AEMでは、コンテンツの構造も同じようになります。 次に、サイトの新しいページを作成します。

1. 前の章で使用したAEM **作成者**&#x200B;サービスにログインします。
1. AEM開始画面で、**サイト**/**WKNDサイト**/**英語**/**記事**&#x200B;をクリックします。
1. 右上隅の&#x200B;**作成**/**ページ**&#x200B;をクリックします。

   ![ページを作成](assets/author-content-publish/create-page-button.png)

   これにより、**ページの作成**&#x200B;ウィザードが表示されます。

1. **記事ページ**&#x200B;テンプレートを選択し、**次へ**&#x200B;をクリックします。

   AEMのページは、ページテンプレートに基づいて作成されます。 ページテンプレートについては、「[ページテンプレート](page-templates.md)」の章で詳しく説明します。

1. **プロパティ**&#x200B;の下に、「Hello World」の&#x200B;**タイトル**&#x200B;を入力します。
1. **名前**&#x200B;を`hello-world`に設定し、**作成**&#x200B;をクリックします。

   ![初期ページのプロパティ](assets/author-content-publish/initial-page-properties.png)

1. ダイアログのポップアップで、「**開く**」をクリックして、新しく作成したページを開きます。

## コンポーネントの作成{#author-component}

AEMコンポーネントは、Webページの小さなモジュラー構成要素と考えることができます。 UIを論理チャンクまたはコンポーネントに分割することで、管理がはるかに容易になります。 コンポーネントを再利用するには、コンポーネントを設定できる必要があります。 これは、作成者ダイアログを通して実行されます。

AEMは、使用する準備が整った[コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)のセットを提供します。 **コアコンポーネント**&#x200B;は、[テキスト](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)や[画像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)などの基本要素から、[カルーセル](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)のような、より複雑なUI要素まで範囲があります。

次に、AEMページエディターを使用してコンポーネントをいくつか作成します。

1. 前の練習で作成した&#x200B;**Hello World**&#x200B;ページに移動します。
1. **編集**&#x200B;モードであることを確認し、左側のパネルで&#x200B;**コンポーネント**&#x200B;アイコンをクリックします。

   ![ページエディターのサイドレール](assets/author-content-publish/page-editor-siderail.png)

   これにより、ページで使用できる使用可能なコンポーネントがコンポーネントライブラリとリストに表示されます。

1. 下にスクロールし、******テキスト(v2)**&#x200B;コンポーネントをドラッグ&amp;ドロップして、ページのメインの編集可能領域に移動します。

   ![テキストコンポーネントをドラッグ&amp;ドロップ](assets/author-content-publish/drag-drop-text-cmp.png)

1. **テキスト**&#x200B;コンポーネントをクリックしてハイライト表示し、**レンチ**&#x200B;アイコン![レンチアイコン](assets/author-content-publish/wrench-icon.png)をクリックして、コンポーネントのダイアログを開きます。 テキストを入力し、変更をダイアログに保存します。

   ![リッチテキストコンポーネント](assets/author-content-publish/rich-text-populated-component.png)

   **テキスト**&#x200B;コンポーネントは、ページ上にリッチテキストを表示する必要があります。

1. **Image(v2)**&#x200B;コンポーネントのインスタンスをページにドラッグする以外、上記の手順を繰り返します。 **画像**&#x200B;コンポーネントのダイアログを開きます。

1. 左側のレールで、**アセット**&#x200B;アイコン![アセットアイコン](assets/author-content-publish/asset-icon.png)をクリックして、**アセットファインダー**&#x200B;に切り替えます。
1. **コンポーネントのダイアログにド** ロパン画像をドラッグし、「 **** 実行」をクリックして変更を保存します。

   ![対追加話するアセット](assets/author-content-publish/add-asset-dialog.png)

1. **タイトル**、**ナビゲーション**、**検索**&#x200B;のように、ページに固定されたコンポーネントがあることを確認してください。 これらの領域はページテンプレートの一部として設定され、個々のページで変更することはできません。 これについては、次の章で詳しく説明します。

他のコンポーネントを自由に試してみてください。 各[コアコンポーネントに関するドキュメントは、](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)を参照してください。 [ページオーサリングに関する詳細なビデオシリーズは、](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)を参照してください。

## 更新を発行{#publish-updates}

AEM環境は、**Author Service**&#x200B;と&#x200B;**Publish Service**&#x200B;に分割されます。 この章では、**作成者サービス**&#x200B;のサイトにいくつかの変更を加えました。 サイト訪問者が表示を行うためには、変更を&#x200B;**発行サービス**&#x200B;に発行する必要があります。

![高レベル図](assets/author-content-publish/author-publish-high-level-flow.png)

*作成者から投稿へのコンテンツの高レベルなフロー*

**1.** コンテンツ作成者がサイトのコンテンツを更新します。アップデートのプレビュー、レビュー、承認を行い、ライブにプッシュすることができます。

**2.**&#x200B;コンテンツが公開されました。公開は、オンデマンドで実行することも、将来の日付に対してスケジュールすることもできます。

**3.** サイト訪問者には、変更が発行サービスに反映されます。

### 変更を公開する

次に、変更を公開します。

1. AEM開始画面で、**サイト**&#x200B;に移動し、**WKNDサイト**&#x200B;を選択します。
1. メニューバーの[**パブリケーションの管理**]をクリックします。

   ![公開を管理](assets/author-content-publish/click-manage-publiciation.png)

   これはまったく新しいサイトなので、すべてのページを公開し、[パブリケーションの管理]ウィザードを使用して、発行する必要のあるページを正確に定義できます。

1. 「**オプション**」の下で、デフォルト設定を「**発行**」のままにし、**今すぐ**&#x200B;にスケジュールします。 「**Next**」をクリックします。
1. 「**スコープ**」で、**WKNDサイト**&#x200B;を選択し、「**子を含める**」をクリックします。 ダイアログで、すべてのボックスのチェックを外します。 完全なサイトを公開したい。

   ![発行範囲の更新](assets/author-content-publish/update-scope-publish.png)

1. 「**発行済み参照**」ボタンをクリックします。 ダイアログで、すべてがチェック済みであることを確認します。 これには、**基本AEMサイトテンプレート**&#x200B;と、サイトテンプレートによって生成されるいくつかの設定が含まれます。 **完了**&#x200B;をクリックして更新します。

   ![参照の発行](assets/author-content-publish/publish-references.png)

1. 最後に、右上隅の「**公開**」をクリックして、コンテンツを公開します。

## 表示が公開したコンテンツ{#publish}

次に、変更を表示する発行サービスに移動します。

1. 発行サービスのURLを簡単に取得するには、作成者のURLをコピーし、`author`の単語を`publish`に置き換えます。 次に例を示します。

   * **作成者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **発行URL** -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. &lt;a0追加/>を公開URLに追加し、最終URLを次のようにします。`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.`/content/wknd.html`

   >[!NOTE]
   >
   > `wknd.html`をサイト名に一致するように変更します（[サイト作成](create-site.md)中に一意の名前を指定した場合）。

1. AEMオーサリング機能を一切使用せずに、サイトにアクセスする必要がある公開URLに移動。

   ![公開済みサイト](assets/author-content-publish/publish-url-update.png)

1. **ナビゲーション**&#x200B;メニューを使用して、**記事**/**Hello World**&#x200B;をクリックし、前に作成したHello Worldページに移動します。
1. **AEM Author Service**&#x200B;に戻り、ページエディターで追加のコンテンツ変更を行います。
1. **ページのプロパティ**&#x200B;アイコン/**ページを公開**&#x200B;をクリックして、これらの変更をページエディター内から直接公開します

   ![直接発行](assets/author-content-publish/page-editor-publish.png)

1. **AEM発行サービス**&#x200B;に戻って、変更を表示します。 ****&#x200B;すぐにはアップデートを見ないでしょう。 これは、**AEM Publish Service**&#x200B;には、Apache WebサーバーとCDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)を介した[キャッシュが含まれているからです。 デフォルトでは、HTMLコンテンツは約5分間キャッシュされます。

1. テスト/デバッグの目的でキャッシュをバイパスするには、`?nocache=true`のようなクエリパラメーターを追加します。 URLは`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`のようになります。 [利用可能なキャッシュ方法と設定の詳細は、](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)を参照してください。

1. また、Cloud Managerの公開サービスのURLも確認できます。 **Cloud Managerプログラム**/**環境**/**環境**&#x200B;に移動します。

   ![表示発行サービス](assets/author-content-publish/view-environment-segments.png)

   「**環境セグメント**」には、**作成者**&#x200B;と&#x200B;**発行**&#x200B;サービスへのリンクがあります。

## これで完了です! {#congratulations}

AEMサイトに変更を作成し、公開しました。

### 次の手順 {#next-steps}

[ページテンプレート](./page-templates.md)の作成および変更方法を説明します。 ページテンプレートとページの関係を理解します。 ページテンプレートのポリシーを設定して、コンテンツの詳細なガバナンスとブランドの一貫性を提供する方法について説明します。  Adobe XDのモックアップに基づいて、構造の整った雑誌記事のテンプレートが作成されます。
