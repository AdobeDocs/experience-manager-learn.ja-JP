---
title: オーサリングと公開の概要 | AEM クイックサイト作成
description: Adobe Experience Manager（AEM）のページエディターを使用して、web サイトのコンテンツを更新します。 オーサリングを容易にするためにコンポーネントを使用する方法について説明します。 AEM オーサー環境とパブリッシュ環境の違いを理解し、ライブサイトに変更を公開する方法を学びます。
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1285'
ht-degree: 100%

---

# オーサリングと公開の概要 {#author-content-publish}

ユーザーが web サイトのコンテンツの更新方法を理解することが重要です。 この章では、**コンテンツ作成者**&#x200B;のペルソナを採用し、前の章で生成されたサイトに編集上の更新を加えます。章の最後では、変更を公開してライブサイトの更新方法を理解します。

## 前提条件 {#prerequisites}

これは複数の部分からなるチュートリアルであり、[サイトの作成](./create-site.md)の章で概説されている手順が完了していることを前提としています。

## 目的 {#objective}

1. AEM Sites の&#x200B;**ページ**&#x200B;および&#x200B;**コンポーネント**&#x200B;の概念を理解します。
1. Web サイトのコンテンツを更新する方法を説明します。
1. ライブサイトに変更を公開する方法を説明します。

## 新しいページを作成します。 {#create-page}

通常、web サイトは複数ページに分割されて、複数ページでのエクスペリエンスを形成します。 AEM はコンテンツを同じ方法で構造化します。 次に、サイトの新しいページを作成します。

1. 前の章で使用した AEM **オーサー**&#x200B;サービスにログインします。
1. AEM 開始画面で、**サイト**／**WKND サイト**／**英語**／**記事**&#x200B;をクリックします。
1. 右上隅で、**作成**／**ページ**&#x200B;をクリックします。

   ![ページを作成](assets/author-content-publish/create-page-button.png)

   **ページを作成**&#x200B;ウィザードが表示されます。

1. **記事ページ**&#x200B;テンプレートを選択し、「**次へ**」をクリックします。

   AEM のページは、ページテンプレートに基づいて作成されます。ページテンプレートについては、[ページ テンプレート](page-templates.md)の章で詳しく説明します。

1. **プロパティ**&#x200B;の下に、「Hello World」という&#x200B;**タイトル**&#x200B;を入力します。
1. **名前**&#x200B;を `hello-world` に設定し、「**作成**」をクリックします。

   ![最初のページのプロパティ](assets/author-content-publish/initial-page-properties.png)

1. ダイアログのポップアップで、「**開く**」をクリックして、新しく作成されたページを開きます。

## コンポーネントのオーサリング {#author-component}

AEM コンポーネントは、web ページの小さなモジュラー構築ブロックと考えることができます。 UI を論理チャンクまたはコンポーネントに分割することで、はるかに管理しやすくなります。 コンポーネントを再利用するには、コンポーネントを設定できる必要があります。 これは、オーサーダイアログを通じて実行します。

AEM は、すぐに使用できる[コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)のセットを提供します。 **コアコンポーネント**&#x200B;は、[テキスト](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=ja)や[画像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=ja)などの基本的な要素から、[カルーセル](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=ja)などのより複雑な UI 要素まで多岐にわたります。

次に、AEM ページエディターを使用して、いくつかのコンポーネントを作成します。

1. 前の演習で作成した **Hello World** ページに移動します。
1. **編集**&#x200B;モードであることを確認し、左側のサイドパネルで&#x200B;**コンポーネント**&#x200B;アイコンをクリックします。

   ![ページエディターのサイドパネル](assets/author-content-publish/page-editor-siderail.png)

   これにより、コンポーネントライブラリが開き、ページで使用できる使用可能なコンポーネントが一覧表示されます。

1. 下にスクロールして、**テキスト（v2）**&#x200B;コンポーネントをページのメインの編集可能領域に&#x200B;**ドラッグ＆ドロップ**&#x200B;します。

   ![テキストコンポーネントをドラッグ＆ドロップ](assets/author-content-publish/drag-drop-text-cmp.png)

1. **テキスト**&#x200B;コンポーネントをクリックして強調表示し、**レンチ**&#x200B;アイコン ![レンチアイコン](assets/author-content-publish/wrench-icon.png) をクリックしてコンポーネントのダイアログを開きます。テキストを入力し、変更内容をダイアログに保存します。

   ![リッチテキストコンポーネント](assets/author-content-publish/rich-text-populated-component.png)

   ページ上の&#x200B;**テキスト**&#x200B;コンポーネントにリッチテキストが表示されるはずです。

1. **画像（v2）**&#x200B;コンポーネントのインスタンスをページ上にドラッグする以外は、上記の手順を繰り返します。**画像**&#x200B;コンポーネントのダイアログを開きます。

1. 左側のパネルで、**アセット**&#x200B;アイコン ![アセットアイコン](assets/author-content-publish/asset-icon.png) をクリックして、**アセットファインダー**&#x200B;に切り替えます。
1. 画像をコンポーネントのダイアログに&#x200B;**ドラッグ＆ドロップ**&#x200B;し、「**完了**」をクリックして変更内容を保存します。

   ![ダイアログへのアセットの追加](assets/author-content-publish/add-asset-dialog.png)

1. ページ上に、**タイトル**、**ナビゲーション**、**検索**&#x200B;などの修正されたコンポーネントがあることを確認します。これらの領域は、ページテンプレートの一部として設定されており、個々のページでは変更できません。これについては、次の章で詳しく説明します。

その他のコンポーネントを自由に試してみてください。各コアコンポーネントのドキュメントについては、[こちら](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)を参照してください。ページのオーサリングに関する詳しいビデオシリーズについては、[こちら](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html?lang=ja)を参照してください。

## 更新内容の公開 {#publish-updates}

AEM 環境は&#x200B;**オーサーサービス**&#x200B;と&#x200B;**パブリッシュサービス**&#x200B;に分かれます。この章では、**オーサーサービス**&#x200B;上のサイトにいくつかの変更を加えました。サイト訪問者に変更内容を表示するには、それを&#x200B;**パブリッシュサービス**&#x200B;に公開する必要があります。

![概要図](assets/author-content-publish/author-publish-high-level-flow.png)

*オーサーからパブリッシュへの大まかなコンテンツフロー*

**1.** コンテンツ作成者がサイトのコンテンツを更新します。更新内容は、プレビュー、レビューおよび承認が済んだ後にライブでプッシュすることができます。

**2.** コンテンツが公開されます。公開は、オンデマンドで実行することも、後日にスケジュールすることもできます。

**3.** 変更内容がパブリッシュサービスに反映されているのがサイト訪問者にわかります。

### 変更内容の公開

次に、変更内容を公開します。

1. AEM 開始画面で **Sites** に移動し、**WKND サイト**&#x200B;を選択します。
1. メニューバーで「**公開を管理**」をクリックします。

   ![公開を管理](assets/author-content-publish/click-manage-publiciation.png)

   これはまったく新しいサイトなので、すべてのページを公開する必要があり、公開を管理ウィザードを使用して、公開する必要があるものを正確に定義します。

1. **オプション**&#x200B;では、**パブリッシュ**&#x200B;の設定をデフォルトのままにし、スケジュールは&#x200B;**今すぐ**&#x200B;に設定します。「**次へ**」をクリックします。
1. **範囲**&#x200B;で **WKND サイト**&#x200B;を選択し、「**子の設定を含める**」をクリックします。ダイアログで、「**子を含める**」のチェックをオンにします。残りのボックスのチェックをオフにして、サイト全体が公開されるようにします。

   ![公開範囲の更新](assets/author-content-publish/update-scope-publish.png)

1. 「**公開済みの参照**」ボタンをクリックします。ダイアログで、すべての項目のチェックがオンになっていることを確認します。これには、**標準サイトテンプレート**&#x200B;や、サイトテンプレートで生成されたいくつかの設定が含まれます。「**完了**」をクリックして更新します。

   ![参照の公開](assets/author-content-publish/publish-references.png)

1. 最後に、**WKND サイト**&#x200B;の横のボックスにチェックを入れ、右上隅の「**次へ**」をクリックします。
1. **ワークフロー**&#x200B;ステップで、「**ワークフロータイトル**」を入力します。これは任意のテキストで構いません。後で、監査証跡の一部として役に立ちます。「初期公開」と入力し、「**公開**」をクリックします。

![ワークフローステップの初期公開](assets/author-content-publish/workflow-step-publish.png)

## 公開済みコンテンツを表示 {#publish}

次に、パブリッシュサービスに移動して、変更を表示します。

1. パブリッシュサービスの URL を容易に取得するには、オーサー URL をコピーして、`author`次の単語`publish`に置き換えます。次に例を示します。

   * **オーサー URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **パブリッシュ URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. パブリッシュ URLに `/content/wknd.html` を追加して、最終的な URL が `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html` のようになるようにします。

   >[!NOTE]
   >
   > [サイト作成](create-site.md)時に、一意の名前を指定した場合は、`wknd.html` をサイト名と一致するように変更します。

1. AEM オーサリング機能を使用せずに、サイトが表示される公開 URL に移動します。

   ![公開済みサイト](assets/author-content-publish/publish-url-update.png)

1. **ナビゲーション**&#x200B;メニューを使い、**記事**／**Hello World** をクリックして、先に作成した Hello World ページに移動してください。
1. **AEM オーサーサービス**&#x200B;に戻り、ページエディターでコンテンツをさらに変更します。
1. **ページプロパティ**&#x200B;アイコン／**ページを公開**&#x200B;をクリックして、ページエディター内で直接、これらの変更を公開します。

   ![直接公開](assets/author-content-publish/page-editor-publish.png)

1. **AEM パブリッシュサービス**&#x200B;に戻って変更を確認します。 おそらく、すぐに更新内容は表示&#x200B;**されない**&#x200B;でしょう。 これは、**AEM パブリッシュサービス**&#x200B;に [Apache web サーバーと CDN を介したキャッシュ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html?lang=ja)が含まれるからです。デフォルトでは、HTML コンテンツは約 5 分間キャッシュされます。

1. テストやデバッグの目的でキャッシュをバイパスするには、`?nocache=true` のようなクエリパラメーターを追加します。 URL は `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true` のようになります。使用可能なキャッシュの戦略と設定の詳細については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html?lang=ja)をご覧ください。

1. また、Cloud Manager でパブリッシュサービスの URL を見つけることもできます。**Cloud Manager プログラム**／**環境**／**環境**&#x200B;に移動します。

   ![パブリッシュサービスの表示](assets/author-content-publish/view-environment-segments.png)

   **環境セグメント**&#x200B;に、**オーサー**&#x200B;および&#x200B;**パブリッシュ**&#x200B;サービスへのリンクがあります。

## おめでとうございます。 {#congratulations}

これで、AEM サイトに対する変更を作成して公開できました。

### 次の手順 {#next-steps}

実際の実装計画では、モックアップと UI のデザインを含むサイトは通常、サイトよりも前に作成されます。 Adobe XD UI キットを使用して、[Adobe XDでの UI 計画](./ui-planning-adobe-xd.md)で Adobe Experience Manager Sites の実装を設計および高速化する方法を理解しておきましょう。 

AEM Sites の機能をさらに確認したい場合は、[ページテンプレート](./page-templates.md)を参照してください。ページテンプレートとページの関係を理解できます。


