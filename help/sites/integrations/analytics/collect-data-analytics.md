---
title: Adobe Analytics でページデータを収集
description: イベント駆動型 Adobe Client Data Layer を使用して、Adobe Experience Manager で構築された web サイトでのユーザーアクティビティに関するデータを収集します。タグルールを使用してこれらのイベントをリッスンし、データをAdobe Analyticsレポートスイートに送信する方法を説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 6a5e62a2a897adc421585e79c5f36f6aa759feaa
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 46%

---

# Adobe Analytics でページデータを収集

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 次を参照してください。 [文書](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 用語の変更を統合的に参照する場合。


の組み込み機能の使用方法を説明します。 [AdobeクライアントデータレイヤーとAEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja) をクリックして、Adobe Experience Manager Sitesのページに関するデータを収集します。 [タグのExperience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja) そして [Adobe Analytics拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ja) は、ページデータをAdobe Analyticsに送信するルールを作成するために使用されます。

## 作成する内容 {#what-build}

![ページデータトラッキング](assets/collect-data-analytics/analytics-page-data-tracking.png)

このチュートリアルでは、Adobeクライアントデータレイヤーのイベントに基づいてトリガールールをタグ付けします。 また、ルールを実行するタイミングの条件を追加し、 **ページ名** および **ページテンプレート** AEM Page からAdobe Analyticsへの値。

### 目的 {#objective}

1. データレイヤーからの変更を取り込むタグプロパティでイベント駆動型ルールを作成する
1. ページデータレイヤーのプロパティをタグプロパティのデータ要素にマッピングする
1. ページビュービーコンを使用したAdobe Analyticsへのページデータの収集と送信

## 前提条件

必要なものは以下のとおりです。

* **タグのプロパティ** Experience Platform
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバーがある。詳しくは、次のドキュメントを参照してください。 [レポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform デバッガー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)ブラウザー拡張機能。 このチュートリアルのスクリーンショットは、Chrome ブラウザーからキャプチャしたものです。
* （オプション）[Adobe Client Data Layer を有効にした](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation) AEM Site。このチュートリアルでは、公開を使用します [WKND](https://wknd.site/us/en.html) サイトを使用している場合でも、独自のサイトを使用することを歓迎します。

>[!NOTE]
>
> タグのプロパティとAEMサイトの統合について [こちらのビデオシリーズ](../experience-platform/data-collection/tags/overview.md)をご覧ください。

## WKND サイトのタグ環境を切り替え

この [WKND](http://wknd.site/us/en.html) は、 [オープンソースプロジェクト](https://github.com/adobe/aem-guides-wknd) 参照として設計され、 [チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja) (AEM実装用 )

AEM環境を設定して WKND コードベースをインストールする代わりに、Experience Platformデバッガーを使用して **スイッチ** 生者 [WKND サイト](http://wknd.site/us/en.html) から *あなたの* タグのプロパティ。 ただし、既に [Adobeクライアントデータレイヤーが有効です](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation).

1. Experience Platformにログインし、 [タグプロパティを作成する](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) （まだの場合）。
1. 最初のタグ JavaScript が [ライブラリが作成されました](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library?lang=ja) タグに昇格しました。 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja).
1. ライブラリの公開先のタグ環境から JavaScript 埋め込みコードをコピーします。

   ![タグプロパティ埋め込みコードをコピー](assets/collect-data-analytics/launch-environment-copy.png)

1. ブラウザーで、新しいタブを開き、に移動します。 [WKND サイト](http://wknd.site/us/en.html)
1. Experience Platform デバッガーブラウザー拡張機能を開きます

   ![Experience Platform デバッガー](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. に移動します。 **Experience Platformタグ** > **設定** および **挿入された埋め込みコード** 既存の埋め込みコードを *あなたの* 手順 3 からコピーした埋め込みコード。

   ![埋め込みコードの置換](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. **コンソールログ**&#x200B;を有効にし、WKND タブでデバッガーを&#x200B;**ロック**&#x200B;します。

   ![コンソールログ](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND サイトでの Adobe Client Data Layer の検証

この [WKND 参照プロジェクト](https://github.com/adobe/aem-guides-wknd) は、AEMコアコンポーネントで構築され、 [Adobeクライアントデータレイヤーが有効です](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation) デフォルトでは。 次に、Adobeクライアントデータレイヤーが有効になっていることを確認します。

1. に移動します。 [WKND サイト](http://wknd.site/us/en.html).
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に移動します。次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   上記のコードは、クライアントデータレイヤーAdobeの現在の状態を返します。

   ![Adobe データレイヤーの状態](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 応答を展開し、`page` エントリを調べます。次のようなデータスキーマが表示されます。

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   ページデータをAdobe Analyticsに送信するには、 `dc:title`, `xdm:language`、および `xdm:template` 」で指定します。

   詳しくは、 [ページスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page?lang=ja) を参照してください。

   >[!NOTE]
   >
   > もし `adobeDataLayer` JavaScript オブジェクトを使用する場合 サイトで [Adobe Client Data Layer が有効になっている](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation)ことを確認してください。

## 「ページの読み込み」ルールの作成

Adobe Client Data Layer は、**イベント**&#x200B;駆動型のデータレイヤーです。AEMページデータレイヤーが読み込まれると、トリガー `cmp:show` イベント。 次の場合にトリガーされるルールを作成 `cmp:show` イベントは、ページデータレイヤーから発生します。

1. Experience Platformに移動し、AEM Site と統合されたタグプロパティに移動します。
1. 次に移動： **ルール** 」セクションに移動し、 **新規ルールの作成**.

   ![ルールを作成](assets/collect-data-analytics/analytics-create-rule.png)

1. ルールに「**ページの読み込み**」という名前を付けます。
1. 内 **イベント** サブセクション、 **追加** 開く **イベント設定** ウィザード。
1. の場合 **イベントタイプ** フィールド、選択 **カスタムコード**.

   ![ルールに名前を付けてカスタムコードイベントを追加する](assets/collect-data-analytics/custom-code-event.png)

1. メインパネルで「**エディターを開く**」をクリックして、次のコードスニペットを入力します。

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   上記のコードスニペットで、データレイヤーに[関数をプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)して、イベントリスナーを追加します。条件 `cmp:show` イベントがトリガーされた `pageShownEventHandler` 関数が呼び出されます。 この関数では、いくつかのサニティチェックが追加され、新しい `event` は、最新の [データレイヤーの状態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) イベントをトリガーしたコンポーネントの

   最後に `trigger(event)` 関数が呼び出されます。 この `trigger()` 関数は、タグプロパティ内の予約名で、 **トリガー** ルール。 この `event` オブジェクトはパラメーターとして渡され、その後、タグプロパティ内の別の予約名で公開されます。 タグプロパティ内のデータ要素は、 `event.component['someKey']`.

1. 変更を保存します。
1. 次に、**アクション**&#x200B;で「**追加**」をクリックして、**アクションの設定**&#x200B;ウィザードを開きます。
1. の場合 **アクションタイプ** フィールド、選択 **カスタムコード**.

   ![カスタムコードアクションタイプ](assets/collect-data-analytics/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   `event` オブジェクトは、カスタムイベントで呼び出された `trigger()` メソッドから渡されます。ここで、 `component` は、データレイヤーから派生した現在のページです `getState` カスタムイベント内。

1. 変更を保存し、 [ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) をタグプロパティに追加して、 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja) をAEM Site で使用している場合にのみ使用できます。

   >[!NOTE]
   >
   > これは、 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 埋め込みコードを **開発** 環境。

1. AEM サイトに移動し、デベロッパーツールを開いてコンソールを表示します。ページを更新すると、コンソールメッセージがログに記録されていることがわかります。

![「ページの読み込み」コンソールメッセージ](assets/collect-data-analytics/page-show-event-console.png)

## データ要素の作成

次に、複数のデータ要素を作成し、Adobe Client Data Layer から様々な値をキャプチャします。前の演習で見たように、カスタムコードを使用して、データレイヤーのプロパティに直接アクセスできます。 データ要素を使用する利点は、複数のタグルールで再利用できる点です。

データ要素は、`@type`、`dc:title`、`xdm:template`の各プロパティにマッピングされています。

### コンポーネントのリソースタイプ

1. Experience Platformに移動し、AEM Site と統合されたタグプロパティに移動します。
1. **データ要素**&#x200B;セクションに移動し、「**新規作成データ要素**」をクリックします。
1. の **名前** フィールドに、 **コンポーネントリソースタイプ**.
1. の **データ要素タイプ** フィールド、選択 **カスタムコード**.

   ![コンポーネントのリソースタイプ](assets/collect-data-analytics/component-resource-type-form.png)

1. クリック **編集画面を開く** ボタンをクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 変更を保存します。

   >[!NOTE]
   >
   > 以下を思い出してください。 `event` オブジェクトは、 **ルール** タグプロパティ内。 データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。したがって、このデータ要素は、 **Page Loaded** 前の手順で作成されたルール *しかし* 他のコンテキストでは安全に使用できません。

### ページ名

1. クリック **データ要素を追加** ボタン
1. の **名前** フィールドに入力 **ページ名**.
1. の **データ要素タイプ** フィールド、選択 **カスタムコード**.
1. クリック **編集画面を開く** ボタンをクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 変更を保存します。

### ページテンプレート

1. 次をクリック： **データ要素を追加** ボタン
1. の **名前** フィールドに入力 **ページテンプレート**.
1. の **データ要素タイプ** フィールド、選択 **カスタムコード**.
1. クリック **編集画面を開く** ボタンをクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. 変更を保存します。

1. これで、ルールの一部として 3 つのデータ要素が揃ったはずです。

   ![ルール内のデータ要素](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 拡張機能の追加

次に、タグプロパティに Analytics 拡張機能を追加し、データをレポートスイートに送信します。

1. Experience Platformに移動し、AEM Site と統合されたタグプロパティに移動します。
1. **拡張機能**／**カタログ**&#x200B;に移動します。
1. **Adobe Analytics** 拡張機能を探し、「**インストール**」をクリックします。

   ![Adobe Analytics 拡張機能](assets/collect-data-analytics/analytics-catalog-install.png)

1. の下 **ライブラリ管理** > **レポートスイート**」で、各タグ環境で使用するレポートスイート id を入力します。

   ![レポートスイート ID を入力します](assets/collect-data-analytics/analytics-config-reportSuite.png)。

   >[!NOTE]
   >
   > このチュートリアルでは、すべての環境で 1 つのレポートスイートを使用しても問題ありませんが、実際の使用では、以下の画像のように別々のレポートスイートを使用することになります

   >[!TIP]
   >
   >アドビでは、 *「自分のライブラリを管理」オプション* ライブラリ管理設定を使用すると、 `AppMeasurement.js` ライブラリを最新の状態に更新しました。

1. チェックボックスをオンにして「**Activity Map を使用**」を有効にします。

   ![「Activity Map を使用」を有効にする](assets/track-clicked-component/analytic-track-click.png)

1. の下 **一般** > **トラッキングサーバー**&#x200B;に設定し、トラッキングサーバーに例えば `tmd.sc.omtrdc.net`. 自社のサイトが `https://` をサポートしている場合は、SSLトラッキングサーバーを入力します

   ![トラッキングサーバーを入力](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 「**保存**」をクリックして、変更を保存します。

## ページロードルールに条件を追加する

次に、**ページロード** ルールを更新し、**コンポーネントリソース タイプ** データ要素を使用して、`cmp:show` イベントが **ページ** のときだけルールが発生するようにします。他のコンポーネントは、 `cmp:show` イベント（例えば、カルーセルコンポーネントは、スライドが変更されると実行します）。 したがって、このルールには条件を追加することが重要です。

1. タグプロパティ UI で、 **Page Loaded** ルールが作成されました。
1. 「**条件**」で「**追加**」をクリックすると、**条件設定**&#x200B;ウィザードが表示されます。
1. の場合 **条件タイプ** フィールド、選択 **値の比較** オプション。
1. フォームフィールドの最初の値を `%Component Resource Type%` に設定します。データ要素アイコン ![data-element icon](assets/collect-data-analytics/cylinder-icon.png) で、**コンポーネント リソースタイプ**&#x200B;データ要素を選択することができます。コンパレーターは `Equals` のままにしてください。
1. 2 番目の値を `wknd/components/page` に設定します。

   ![「ページの読み込み」ルールの条件設定](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > この条件は、チュートリアルの前半で作成した `cmp:show` イベントを監視するカスタムコード関数内に追加することが可能です。しかし、UI 内に追加することで、ルールの変更を必要とする可能性のある追加ユーザーに対して、可視性を高めることができます。さらに、データ要素を使用できます。

1. 変更を保存します。

## Analytics 変数の設定とページビュービーコンのトリガー

現在、**ページの読み込み**&#x200B;ルールは、単にコンソールステートメントを出力します。次に、データ要素と Analytics 拡張機能を使用して、**ページの読み込み**&#x200B;ルールの&#x200B;**アクション**&#x200B;として Analytics 変数を設定します。また、をトリガーする追加のアクションを設定します **ページビュービーコン** 収集したデータをAdobe Analyticsに送信します。

1. 「Page Loaded」ルールで、 **削除** の **コア — カスタムコード** アクション（コンソールステートメント）:

   ![「カスタムコードを削除」アクション](assets/collect-data-analytics/remove-console-statements.png)

1. 「アクション」サブセクションで、「 **追加** をクリックして新しいアクションを追加します。

1. **拡張機能**&#x200B;の種類を **Adobe Analytics**、**アクションタイプ**&#x200B;を「**変数の設定**」に設定します。

   ![アクション拡張機能を Analytics 設定変数に設定](assets/collect-data-analytics/analytics-set-variables-action.png)

1. メインパネルで、使用可能な **eVar** データ要素の値として設定します。 **ページテンプレート**. 「データ要素」アイコン![「データ要素」アイコン](assets/collect-data-analytics/cylinder-icon.png)で、**ページテンプレート**&#x200B;要素を選択します。

   ![eVar ページテンプレートとして設定](assets/collect-data-analytics/set-evar-page-template.png)

1. 下にスクロールして、**追加設定**&#x200B;で、**ページ名**&#x200B;をデータ要素&#x200B;**ページ名**&#x200B;に設定します。

   ![ページ名環境変数セット](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 変更を保存します。

1. 次に、「 **Adobe Analytics — 変数を設定** をタップすることで **プラス** アイコン：

   ![タグルールアクションを追加](assets/collect-data-analytics/add-additional-launch-action.png)

1. **拡張機能**&#x200B;の種類を **Adobe Analytics**、**アクションタイプ**&#x200B;を「**ビーコンの送信**」に設定します。このアクションはページビューと見なされるので、デフォルトのトラッキング設定は「 **`s.t()`**.

   ![ビーコンの送信 Adobe Analytics アクション](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 変更を保存します。**ページの読み込み**&#x200B;ルールは、次のような構成になります。

   ![最終タグルール設定](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.**`cmp:show` イベントをリッスンします。
   * **2.** イベントがページによってトリガーされたことを確認します。
   * **3.****ページ名**&#x200B;と&#x200B;**ページテンプレート**&#x200B;に Analytics の変数を設定する
   * **4.** Analytics ページビュービーコンの送信

1. すべての変更を保存し、タグライブラリを構築して、適切な環境に昇格します。

## ページビュービーコンと Analytics 呼び出しの検証

**ページの読み込み**&#x200B;ルールが Analytics ビーコンを送信するようになったので、Experience Platform Debugger を使用して Analytics トラッキング変数を確認できます。

1. ブラウザーで [WKND サイト](https://wknd.site/us/en.html)を開きます。
1. ![「Experience Platform Debugger」アイコン](assets/collect-data-analytics/experience-cloud-debugger.png) の「デバッガー」アイコンをクリックして Experience Platform Debugger を起動します。
1. Debugger がタグプロパティをにマッピングしていることを確認します。 *あなたの* 開発環境（前述のとおり） **コンソールログ** がオンになっている。
1. Analytics メニューを開き、レポートスイートが&#x200B;*自身の*&#x200B;レポートスイートに設定されていることを確認します。「ページ名」も入力する必要があります。

   ![「Analytics」タブのデバッガー](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 下にスクロールして、**ネットワークリクエスト**&#x200B;を展開します。**ページテンプレート**&#x200B;に設定されている&#x200B;**eVar**&#x200B;を確認できます。

   ![eVar とページ名を設定](assets/collect-data-analytics/evar-page-name-set.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。ページ上部で&#x200B;**カルーセル**&#x200B;をクリックスルーします。

   ![カルーセルページをクリックスルー](assets/collect-data-analytics/click-carousel-page.png)

1. ブラウザーコンソールで、コンソールステートメントを確認します。

   ![条件が満たされていません](assets/collect-data-analytics/condition-not-met.png)

   これは、カルーセルが `cmp:show` イベントをトリガーする&#x200B;*が*、**コンポートリソースタイプ**&#x200B;のチェックにより、イベントが発生しないためです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、 **コンソールログ** は以下でチェックされています **Experience Platformタグ** (Experience PlatformDebugger)

1. 「[西オーストラリア](https://wknd.site/us/en/magazine/western-australia.html)」などの記事ページに移動します。「ページ名」と「テンプレートタイプ」が変更されるのを確認します。

## これで完了です。

Experience Platformでイベントドリブン型Adobeクライアントデータレイヤーとタグを使用して、AEM Site からデータページデータを収集し、Adobe Analyticsに送信したのです。

### 次の手順

イベント駆動型の Adobe Client Data Layer を使用して、[Adobe Experience Manager サイト](track-clicked-component.md)上の特定のコンポーネントのクリックを追跡する方法については、次のチュートリアルを参照してください。
