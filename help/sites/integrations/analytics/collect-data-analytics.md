---
title: Adobe Analytics タグ拡張機能を使用した AEM Sites と Adobe Analytics の統合
description: イベント駆動型 Adobe Client Data Layer を使用して、AEM Sites と Adobe Analytics を統合し、Adobe Experience Manager で構築された web サイトでのユーザーアクティビティに関するデータを収集します。タグルールを使用してこれらのイベントをリッスンし、データを Adobe Analytics レポートスイートに送信する方法を説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="統合" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 668
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 100%

---

# AEM Sites と Adobe Analytics の統合

Adobe Analytics タグ拡張機能を使用して AEM Sites と Adobe Analytics を統合し、[AEM コアコンポーネントと共に Adobe Client Data Layer](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja) の組み込み機能を使用して、Adobe Experience Manager Sites 内のページに関するデータを収集する方法を説明します。[Experience Platform のタグ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)と [Adobe Analytics 拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ja)を使用して、ページデータを Adobe Analytics に送信するためのルールを作成します。

## 作成するもの {#what-build}

![ページデータトラッキング](assets/collect-data-analytics/analytics-page-data-tracking.png)

このチュートリアルでは、Adobe Client Data Layer からのイベントに基づいてタグルールをトリガーします。また、ルールを起動するタイミングの条件を追加したあと、AEM ページの&#x200B;**ページ名**&#x200B;と&#x200B;**ページテンプレート**&#x200B;の値を Adobe Analytics に送信します。

### 目的 {#objective}

1. 変更内容をデータレイヤーからキャプチャするイベント駆動型ルールをタグプロパティに作成します
1. ページデータレイヤーのプロパティをタグプロパティのデータ要素にマッピングします
1. ページデータを収集し、ページビュービーコンを使用して Adobe Analytics に送信します

## 前提条件

必要なものは以下のとおりです。

* Experience Platform の&#x200B;**タグプロパティ**
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバー。[レポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=ja)については、次のドキュメントを参照してください。
* [Experience Platform デバッガー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja)ブラウザー拡張機能。 このチュートリアルのスクリーンショットは、Chrome ブラウザーからキャプチャしたものです。
* （オプション）[Adobe Client Data Layer を有効にした](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation) AEM Site。このチュートリアルでは、公開されている [WKND](https://wknd.site/us/en.html) サイトを使用しますが、独自のサイトを使用してもかまいません。

>[!NOTE]
>
> タグプロパティと AEM サイトの統合について不明な点がある場合は、[こちらのビデオシリーズ](../experience-platform/data-collection/tags/overview.md)をご覧ください。

## WKND サイトに向けたタグ環境の切り替え

[WKND](https://wknd.site/us/en.html) は、[オープンソースプロジェクト](https://github.com/adobe/aem-guides-wknd)に基づいて構築された公開サイトであり、AEM 実装の参考用および[チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja)として設計されています。

AEM 環境をセットアップして WKND コードベースをインストールする代わりに、Experience Platform デバッガーを使用して、ライブの [WKND Site](https://wknd.site/us/en.html) を&#x200B;*お使いの*&#x200B;タグプロパティに&#x200B;**切り替える**&#x200B;ことができます。ただし、既に [Adobe Client Data Layer が有効になっている](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation)場合は、独自の AEM サイトを使用できます。

1. Experience Platform にログインし、[タグプロパティを作成](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ja)します（まだの場合）。
1. 初期のタグ JavaScript [ライブラリが作成され](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html?lang=ja#create-a-library)、タグ[環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja)に昇格されたことを確認します。
1. ライブラリの公開先のタグ環境から JavaScript 埋め込みコードをコピーします。

   ![タグプロパティ埋め込みコードのコピー](assets/collect-data-analytics/launch-environment-copy.png)

1. ブラウザーで新しいタブを開き、[WKND Site](https://wknd.site/us/en.html) に移動します。
1. Experience Platform デバッガーブラウザー拡張機能を開きます

   ![Experience Platform デバッガー](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. **Experience Platform タグ**／**設定**&#x200B;に移動し、「**挿入された埋め込みコード**」に示されている既存の埋め込みコードを、手順 3 からコピーした&#x200B;*お使いの*&#x200B;埋め込みコードに置き換えます。

   ![埋め込みコードの置換](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. **コンソールログ**&#x200B;を有効にし、WKND タブでデバッガーを&#x200B;**ロック**&#x200B;します。

   ![コンソールログ](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND サイトでの Adobe Client Data Layer の検証

[WKND 参照プロジェクト](https://github.com/adobe/aem-guides-wknd)は AEM コアコンポーネントで構築されており、デフォルトで [Adobe Client Data Layer が有効](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation)になっています。次に、Adobe Client Data Layer が有効になっていることを確認します。

1. [WKND Site](https://wknd.site/us/en.html) に移動します。
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に移動します。次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   上記のコードは、Adobe Client Data Layer の現在の状態を返します。

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

   ページデータを Adobe Analytics に送信するには、データレイヤーの `dc:title`、`xdm:language`、`xdm:template` などの標準プロパティを使用します。

   詳しくは、コアコンポーネントデータスキーマの[ページスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#page)を参照してください。

   >[!NOTE]
   >
   > `adobeDataLayer` JavaScript オブジェクトが表示されない場合は、サイトで [Adobe Client Data Layer が有効になっている](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation)ことを確認してください。

## 「ページの読み込み」ルールの作成

Adobe Client Data Layer は、**イベント駆動型**&#x200B;のデータレイヤーです。AEM ページデータレイヤーが読み込まれると、`cmp:show` イベントがトリガーされます。`cmp:show` イベントがページデータレイヤーから発生したときにトリガーされるルールを作成します。

1. Experience Platform に移動し、AEM サイトと統合されているタグプロパティに移動します。
1. タグプロパティ UI の「**ルール**」セクションに移動し、「**新しいルールを作成**」をクリックします。

   ![ルールを作成](assets/collect-data-analytics/analytics-create-rule.png)

1. ルールに「**ページの読み込み**」という名前を付けます。
1. 「**イベント**」サブセクションで「**追加**」をクリックして、**イベント設定**&#x200B;ウィザードを開きます。
1. 「**イベントタイプ**」フィールドで、「**カスタムコード**」を選択します。

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

   上記のコードスニペットで、データレイヤーに[関数をプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)して、イベントリスナーを追加します。`cmp:show` イベントがトリガーされると、`pageShownEventHandler` 関数が呼び出されます。この関数では、いくつかの健全性チェックが追加され、イベントをトリガーしたコンポーネントの[データレイヤーの最新状態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)で新しい `event` が作成されます。

   最後に、`trigger(event)` 関数が呼び出されます。この `trigger()` 関数は、タグプロパティ内の予約名で、ルールを&#x200B;**トリガー**&#x200B;します。`event` オブジェクトはパラメーターとして渡され、タグプロパティ内の別の予約名で公開されます。タグプロパティ内のデータ要素は、`event.component['someKey']` などのコードスニペットを使用して様々なプロパティを参照できるようになりました。

1. 変更を保存します。
1. 次に、**アクション**&#x200B;で「**追加**」をクリックして、**アクションの設定**&#x200B;ウィザードを開きます。
1. 「**アクションタイプ**」フィールドで、「**カスタムコード**」を選択します。

   ![カスタムコードアクションタイプ](assets/collect-data-analytics/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   `event` オブジェクトは、カスタムイベントで呼び出された `trigger()` メソッドから渡されます。ここで、`component` はカスタムイベントでデータレイヤーに対する `getState` から得られる現在のページです。

1. 変更を保存し、タグプロパティで[ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=ja)を実行して、AEM サイトで使用する[環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja)にコードを昇格します。

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja) を使用して埋め込みコードを&#x200B;**開発**&#x200B;環境に切り替えると、便利なことがあります。

1. AEM サイトに移動し、デベロッパーツールを開いてコンソールを表示します。ページを更新すると、コンソールメッセージがログに記録されていることがわかります。

![「ページの読み込み」コンソールメッセージ](assets/collect-data-analytics/page-show-event-console.png)

## データ要素の作成

次に、複数のデータ要素を作成し、Adobe Client Data Layer から様々な値をキャプチャします。前の演習で見たように、カスタムコードを使用してデータレイヤーのプロパティに直接アクセスすることができます。データ要素を使用する利点は、タグルール全体で再利用できることです。

データ要素は、`@type`、`dc:title`、`xdm:template`の各プロパティにマッピングされています。

### コンポーネントのリソースタイプ

1. Experience Platform に移動し、AEM サイトと統合されているタグプロパティに移動します。
1. 「**データ要素**」セクションに移動し、「**新しいデータ要素を作成**」をクリックします。
1. 「**名前**」フィールドに「**コンポーネントのリソースタイプ**」と入力します。
1. 「**データ要素タイプ**」フィールドで「**カスタムコード**」を選択します。

   ![コンポーネントのリソースタイプ](assets/collect-data-analytics/component-resource-type-form.png)

1. 「**エディターを開く**」ボタンをクリックし、カスタムコードエディターで以下を入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 変更を保存します。

   >[!NOTE]
   >
   > `event` オブジェクトは、タグプロパティの&#x200B;**ルール**&#x200B;をトリガーしたイベントに基づいて利用可能になり、スコープが設定されることを思い出してください。データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。したがって、このデータ要素は、前のステップで作成された「**ページの読み込み**」ルールなどのルール内で安全に使用できます&#x200B;*が*、他のコンテキストでは安全に使用できません。

### ページ名

1. 「**データ要素を追加**」ボタンをクリックします。
1. 「**名前**」フィールドに「**ページ名**」と入力します。
1. 「**データ要素タイプ**」フィールドで「**カスタムコード**」を選択します。
1. 「**エディターを開く**」ボタンをクリックし、カスタムコードエディターで以下を入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 変更を保存します。

### ページテンプレート

1. 「**データ要素を追加**」ボタンをクリックします。
1. 「**名前**」フィールドに「**ページテンプレート**」と入力します。
1. 「**データ要素タイプ**」フィールドで「**カスタムコード**」を選択します。
1. 「**エディターを開く**」ボタンをクリックし、カスタムコードエディターで以下を入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. 変更を保存します。

1. これで、ルールの一部として 3 つのデータ要素が揃ったはずです。

   ![ルール内のデータ要素](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 拡張機能の追加

次に、データをレポートスイートに送信する Analytics 拡張機能をタグプロパティに追加します。

1. Experience Platform に移動し、AEM サイトと統合されているタグプロパティに移動します。
1. **拡張機能**／**カタログ**&#x200B;に移動します。
1. **Adobe Analytics** 拡張機能を探し、「**インストール**」をクリックします。

   ![Adobe Analytics 拡張機能](assets/collect-data-analytics/analytics-catalog-install.png)

1. **ライブラリ管理**／**レポートスイート**&#x200B;で、各タグ環境で使用するレポートスイート ID を入力します。

   ![レポートスイート ID を入力します](assets/collect-data-analytics/analytics-config-reportSuite.png)。

   >[!NOTE]
   >
   > このチュートリアルでは、すべての環境で 1 つのレポートスイートを使用しても問題ありませんが、実際の使用では、以下の画像のように別々のレポートスイートを使用することになります

   >[!TIP]
   >
   >ライブラリ管理設定として「*ライブラリをシスで管理*」オプションを使用すると、`AppMeasurement.js` ライブラリを最新の状態に保つことがはるかに容易になるので、このオプションの使用をお勧めします。

1. チェックボックスをオンにして「**Activity Map を使用**」を有効にします。

   ![「Activity Map を使用」を有効にする](assets/track-clicked-component/analytic-track-click.png)

1. **一般**／**トラッキングサーバー**&#x200B;に、お使いのトラッキングサーバー（例：`tmd.sc.omtrdc.net`）を入力します。自社のサイトが `https://` をサポートしている場合は、SSLトラッキングサーバーを入力します

   ![トラッキングサーバーを入力](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 「**保存**」をクリックして、変更を保存します。

## ページロードルールに条件を追加する

次に、**ページロード** ルールを更新し、**コンポーネントリソース タイプ** データ要素を使用して、`cmp:show` イベントが **ページ** のときだけルールが発生するようにします。他のコンポーネントで `cmp:show` イベントを発生させることができます。例えば、カルーセルコンポーネントでスライドの変更時に発生します。したがって、このルールの条件を追加することが重要です。

1. タグプロパティ UIで、先ほど作成した&#x200B;**ページの読み込み**&#x200B;ルールに移動します。
1. 「**条件**」で「**追加**」をクリックすると、**条件設定**&#x200B;ウィザードが開きます。
1. 「**条件タイプ**」フィールドで「**値比較**」オプションを選択します。
1. フォームフィールドの最初の値を `%Component Resource Type%` に設定します。データ要素アイコン ![data-element icon](assets/collect-data-analytics/cylinder-icon.png) で、**コンポーネント リソースタイプ**&#x200B;データ要素を選択することができます。コンパレーターは `Equals` のままにしてください。
1. 2 番目の値を `wknd/components/page` に設定します。

   ![「ページの読み込み」ルールの条件設定](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > この条件は、チュートリアルの前半で作成した `cmp:show` イベントを監視するカスタムコード関数内に追加することが可能です。しかし、UI 内に追加することで、ルールの変更を必要とする可能性のある追加ユーザーに対して、可視性を高めることができます。さらに、データ要素を使用できます。

1. 変更を保存します。

## Analytics 変数の設定とページビュービーコンのトリガー

現在、**ページの読み込み**&#x200B;ルールは、単にコンソールステートメントを出力します。次に、データ要素と Analytics 拡張機能を使用して、**ページの読み込み**&#x200B;ルールの&#x200B;**アクション**&#x200B;として Analytics 変数を設定します。また、**ページビュービーコン**&#x200B;をトリガーし、収集したデータを Adobe Analytics に送信するアクションを追加で設定します。

1. 「ページの読み込み」ルールで、**コア - カスタムコード**&#x200B;アクション（コンソールステートメント）を&#x200B;**削除**&#x200B;します。

   ![カスタムコードアクションの削除](assets/collect-data-analytics/remove-console-statements.png)

1. 「アクション」サブセクションで、「**追加**」をクリックして新しいアクションを追加します。

1. **拡張機能**&#x200B;の種類を **Adobe Analytics**、**アクションタイプ**&#x200B;を「**変数の設定**」に設定します。

   ![アクション拡張機能を Analytics 設定変数に設定](assets/collect-data-analytics/analytics-set-variables-action.png)

1. メインパネルで、使用可能な **eVar** を選択し、データ要素&#x200B;**ページテンプレート**&#x200B;の値として設定します。「データ要素」アイコン![「データ要素」アイコン](assets/collect-data-analytics/cylinder-icon.png)で、**ページテンプレート**&#x200B;要素を選択します。

   ![eVar ページテンプレートとして設定](assets/collect-data-analytics/set-evar-page-template.png)

1. 下にスクロールして、**追加設定**&#x200B;で、**ページ名**&#x200B;をデータ要素&#x200B;**ページ名**&#x200B;に設定します。

   ![ページ名環境変数セット](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 変更を保存します。

1. 次に、**プラス**&#x200B;アイコンをタップして、**Adobe Analytics - 変数を設定**&#x200B;の右側にさらにアクションを追加します。

   ![タグルールアクションの追加](assets/collect-data-analytics/add-additional-launch-action.png)

1. 「**拡張機能**」タイプを「**Adobe Analytics**」に設定し、「**アクションタイプ**」を「**ビーコンの送信**」に設定します。このアクションはページビューと見なされるので、「トラッキング」の設定はデフォルトの **`s.t()`** のままにしておきます。

   ![ビーコンの送信 Adobe Analytics アクション](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 変更を保存します。**ページの読み込み**&#x200B;ルールは、次のような構成になります。

   ![最終的なタグルール設定](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.**`cmp:show` イベントをリッスンします。
   * **2.** イベントがページによってトリガーされたことを確認します。
   * **3.****ページ名**&#x200B;と&#x200B;**ページテンプレート**&#x200B;に Analytics の変数を設定する
   * **4.** Analytics ページビュービーコンの送信

1. すべての変更を保存し、タグライブラリを構築して、適切な環境に昇格します。

## ページビュービーコンと Analytics 呼び出しの検証

**ページの読み込み**&#x200B;ルールが Analytics ビーコンを送信するようになったので、Experience Platform Debugger を使用して Analytics トラッキング変数を確認できます。

1. ブラウザーで [WKND サイト](https://wknd.site/us/en.html)を開きます。
1. ![「Experience Platform Debugger」アイコン](assets/collect-data-analytics/experience-cloud-debugger.png) の「デバッガー」アイコンをクリックして Experience Platform Debugger を起動します。
1. 前述のように、Debugger がタグプロパティを&#x200B;*ご自身の*&#x200B;開発環境にマッピングし、**コンソールログ**&#x200B;がオンになることを確認します。
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
   > コンソールログが表示されない場合は、Experience Platform Debugger の **Experience Platform タグ**&#x200B;で「**コンソールログ**」がオンになっていることを確認します。

1. 「[西オーストラリア](https://wknd.site/us/en/magazine/western-australia.html)」などの記事ページに移動します。「ページ名」と「テンプレートタイプ」が変更されるのを確認します。

## おめでとうございます。

イベント駆動型の Adobe Client Data Layer と Experience Platform のタグを使用して、AEM Sites からデータページデータを収集し、Adobe Analytics に送信しました。

### 次の手順

イベント駆動型の Adobe Client Data Layer を使用して、[Adobe Experience Manager サイト](track-clicked-component.md)上の特定のコンポーネントのクリックを追跡する方法については、次のチュートリアルを参照してください。
