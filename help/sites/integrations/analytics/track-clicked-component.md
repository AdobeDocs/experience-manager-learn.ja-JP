---
title: Adobe Analyticsでクリックされたコンポーネントを追跡
description: イベント主導型のAdobeクライアントデータレイヤーを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡します。 Experience Platform Launchのルールを使用して、これらのイベントをリッスンし、リンクトラッキングビーコンと共にデータをAdobe Analyticsに送信する方法について説明します。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 4%

---


# Adobe Analyticsでクリックされたコンポーネントを追跡

Use the event-driven [Adobe Client Data Layer with AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html) to track clicks of specific components on an Adobe Experience Manager site. Experience Platform Launch でルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作成する内容

WKNDマーケティングチームは、ホームページでどの誘い文句(CTA)ボタンが最も高いパフォーマンスを発揮しているかを把握したいと考えています。 このチュートリアルでは、Teaser `cmp:click` および **Buttonコンポーネントの****** イベントをリッスンし、コンポーネントIDと新しいイベントをトラックリンクビーコンと共にAdobe Analyticsに送信する新しいルールをExperience Platform Launchに追加します。

![クリック追跡の対象](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. イベントに基づいて、「起動」でイベント主導のルールを作成し `cmp:click` ます。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. クリックされたコンポーネントIDを設定し、イベントAdobe Analyticsをリンクトラッキングビーコンと共に送信します。

## 前提条件

このチュートリアルは、Adobe Analyticsとの [ページデータ収集の継続で](./collect-data-analytics.md) 、次の点を前提としています。

* **Adobe Analytics拡張機能が有効な** 起動プロパティ [](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)
* **Adobe Analytics** test/devレポートスイートIDとトラッキングサーバー。 新しいレポートスイートを [作成するには、次のドキュメントを参照してください](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。
* [https://wknd.site/us/en.htmlに読み込まれた起動プロパティを使用して設定されたExperience Platformデバッガ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) ーのブラウザー拡張機能、 [](https://wknd.site/us/en.html) またはAEMデータレイヤーが有効なAdobeサイト。

## Inspectザボタンアンドティーザースキーマ

「起動」でルールを作成する前に、ボタンとTeaserの [スキーマを確認し](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) 、データレイヤー実装でそれらを調べると便利です。

1. https://wknd.site/us/en.htmlに移動し [ます。](https://wknd.site/us/en.html)
1. ブラウザの開発者ツールを開き、 **コンソールに移動します**。 次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   Adobeクライアントデータレイヤーの現在の状態を返します。

   ![ブラウザーコンソールを使用したデータレイヤーの状態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 応答を展開し、プリフィックス `button-` とエントリが付いたエントリを探し `teaser-xyz-cta` ます。 次のようなデータスキーマが表示されます。

   ボタンスキーマ:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaserスキーマ:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "nt:unstructured"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   これらは、 [コンポーネント/コンテナ項目スキーマに基づきます](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)。 「開始」で作成するルールは、このスキーマを使用します。

## CTAクリック済みルールの作成

Adobe・クライアント・データ・レイヤーは、 **イベント** 駆動データ・レイヤーです。 コアコンポーネントがクリックされると、 `cmp:click` イベントがデータレイヤーを介してディスパッチされます。 次に、 `cmp:click` イベントをリッスンするルールを作成します。

1. Experience Platform Launchに移動し、AEM Siteに統合されたWebプロパティに移動します。
1. 「UIの起動」の「 **ルール** 」セクションに移動し、「ルー **ル**」をクリックします。
1. ルールに「 **CTA Clicked**」という名前を付けます。
1. 「 **イベント** / **追加** 」をクリックして、 **イベント設定** ウィザードを開きます。
1. 「 **イベントタイプ** 」で「 **カスタムコード**」を選択します。

   ![ルールに「CTA Clicked」という名前を付け、カスタムコードイベントを追加します。](assets/track-clicked-component/custom-code-event.png)

1. メインパネルで **「エディターを開く** 」をクリックし、次のコードスニペットを入力します。

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   上記のコードスニペットでは、関数をデータレイヤーに [プッシュしてイベントリスナーを追加します](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 。 イベントがトリガされると、 `cmp:click` 関数が呼び出され `componentClickedHandler` ます。 この関数では、いくつかのサニティチェックが追加され、イベントをトリガーしたコンポーネントのデータレイヤーの最新 `event` 状態で新しい [](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) オブジェクトが構築されます。

   その後 `trigger(event)` に。 `trigger()` は、起動で予約された名前で、起動ルールを「トリガー」します。 この `event` オブジェクトをパラメータとして渡し、その後、Launch内の別の予約名で公開され `event`ます。 起動のデータ要素は、次のような様々なプロパティを参照できるようになりました。 `event.component['someKey']`.

1. 変更内容を保存します。
1. 「 **アクション** 」の次に **、「** 追加」をクリックして「 **アクションの設定** 」ウィザードを開きます。
1. 「 **アクションタイプ** 」で、「 **カスタムコード**」を選択します。

   ![カスタムコードアクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで **「エディターを開く** 」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   この `event` オブジェクトは、カスタムイベントで呼び出される `trigger()` メソッドから渡されます。 `component` は、クリックをトリガーしたデータレイヤーから派生したコンポーネント `getState` の現在の状態です。

1. 変更を保存し、「起動」で [ビルドを実行して](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) 、コードをAEMサイトで [](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/publish/environments.html) 使用されている環境に移行します。

   >[!NOTE]
   >
   > 埋め込みコードを [開発](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 環境に切り替えるには、 **** Adobe Experience Platformデバッガを使用すると非常に便利です。

1. WKND [サイトに移動し](https://wknd.site/us/en.html) 、開発者ツールを開いてコンソールを表示します。 「 **ログを保存**」を選択します。

1. 「 **Teaser** 」ボタンまたは「 **Button** CTA」ボタンの1つをクリックして、別のページに移動します。

   ![CTAボタンをクリック](assets/track-clicked-component/cta-button-to-click.png)

1. 開発者コンソールで、 **CTA Clicked** ルールが実行されたことを確認します。

   ![CTAボタンがクリックされました](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネントIDとタイトルを取り込みます。 前の練習で思い出したとおり、の出力 `event.path` はに似 `component.button-b6562c963d` たもので、の値は「表示旅行」 `event.component['dc:title']` のようなものでした。

### コンポーネントID

1. Experience Platform Launchに移動し、AEM Siteに統合されたWebプロパティに移動します。
1. 「 **データ要素** 」セクションに移動し、「 **追加新規データ要素**」をクリックします。
1. [ **名前** ]に **、** Component IDと入力します。
1. 「 **データ要素のタイプ** 」で「 **カスタムコード**」を選択します。

   ![コンポーネントIDデータ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. 「 **エディターを開く** 」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   変更内容を保存します。

   >[!NOTE]
   >
   > オブジェクトが使用可能になり、起動時にルー `event` ルをトリガーしたイベントに基づいてスコープされることを忘れないで **ください** 。 データ要素の値は、データ要素がルール内で *参照されるまで設定されません* 。 したがって、前の練習で作成した **CTA Clicked** (クリック済み *CTA)ルールのように、ルール内でこのデータ要素を使用しても安全ですが、他のコンテキストでは安全に使用できません* 。

### コンポーネントのタイトル

1. 「 **データ要素** 」セクションに移動し、「 **追加新規データ要素**」をクリックします。
1. [ **名前** ]に **、** Component Titleと入力します。
1. 「 **データ要素のタイプ** 」で「 **カスタムコード**」を選択します。
1. 「 **エディターを開く** 」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   変更内容を保存します。

## CTA追加クリックルールの条件

次に、「 **CTA Clicked** 」ルールを更新して、Teaser `cmp:click` または **Buttonに対して** イベントが起動した場合にのみルールが起動されるようにします ****。 TeaserのCTAはデータレイヤー内の別のオブジェクトと見なされるので、親をチェックしてTeaserから来ているかどうかを確認することが重要です。

1. 起動UIで、前に作成した **ページが読み込まれた** 」ルールに移動します。
1. 「 **条件** 」の下の「 **追加** 」をクリックして、 **条件の設定** ウィザードを開きます。
1. 「 **条件タイプ** 」で「 **カスタムコード**」を選択します。

   ![CTAクリック条件カスタムコード](assets/track-clicked-component/custom-code-condition.png)

1. 「 **エディターを開く** 」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       //Check for Button Type
       if(event.component['@type'] === 'wknd/components/button') {
           return true;
       } else if (event.component['@type'] == 'nt:unstructured') {
           // Check for CTA inside a Teaser
           var parentComponentId = event.component['parentId'];
           var parentComponent = window.adobeDataLayer.getState('component.' + parentComponentId);
   
           if(parentComponent['@type'] === 'wknd/components/teaser') {
               return true;
           }
       }
   }
   
   return false;
   ```

   上記のコードは、最初にリソースタイプが **Button** （ボタン）からのものかどうかを調べ、次にリソースタイプがTeaser内のCTAからのものかどうかを調べ **ます**。

1. 変更内容を保存します。

## Analytics変数の設定とリンク追跡ビーコンのトリガー

現在、 **CTA Clicked** ルールは、単にコンソールステートメントを出力するだけです。 次に、データ要素とAnalytics拡張機能を使用して、Analytics変数を **アクションとして設定します**。 また、リンクの **追跡をトリガーし、収集したデータをAdobe Analyticsに送信する追加のアクションも設定します** 。

1. 「 **Page Loaded** 」ルールで **、「** Core - Custom Code **** （コンソール文）」アクションを削除します。

   ![カスタムコードアクションの削除](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」の下のをクリックし **** 追加て、新しいアクションを追加します。
1. 「 **Extension** type( **拡張子** )」を「 **Adobe Analytics** 」に設定し、「 **Action Type（アクションタイプ）」を「Set Variables（変数を設定）」**&#x200B;に設定します。

1. eVar **、** Prop **、**&#x200B;イベントに次の値を設定します ****。

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVarpropとイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここ `%Component ID%` は、クリックされたCTAの一意の識別子を取得するので使用されます。 を使用すると、Analyticsレポート `%Component ID%` に次のような値が含まれる可能性があり `button-2e6d32893a`ます。 を使用す `%Component Title%` ると、よりわかりやすい名前が付けられますが、値が一意でない可能性があります。

1. 次に、 **Adobe Analyticsの右側に「アクション — 変数を設定** 」を追加します。次に、 **プラス** アイコンをタップします。

   ![追加追加の開始アクション](assets/track-clicked-component/add-additional-launch-action.png)

1. 「 **Extension** type( **拡張のタイプ)」を** 「 **Adobe Analytics** 」に設定し、「Action Type（アクションのタイプ） **」を「Send Beacon**」に設定します。
1. 「 **トラッキング** 」で、ラジオボタンをに設定し **`s.tl()`**&#x200B;ます。
1. 「 **リンクタイプ** 」で「 **カスタムリンク** 」を選択し、「 **リンク名** 」で値を次に設定します。 **`%Component Title%: CTA Clicked`**:

   ![リンクビーコンの送信の設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   これにより、データ要素の **コンポーネントタイトル** と静的文字列の **CTA Clicked**.の動的変数が組み合わされます。

1. 変更内容を保存します。これで、 **CTA Clicked** ruleの設定は次のようになります。

   ![最終起動の設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** イベントをリッスンし `cmp:click` ます。
   * **2.** イベントが **Button** または **Teaserでトリガーされたことを確認します**。
   * **3.** コンポー **ネントIDを** eVar **、** prop **、******&#x200B;イベントとして追跡するためのAnalytics変数を設定します。
   * **4.** Analyticsリンク追跡ビーコンを送信します(ページ表示 **として扱わない** )。

1. すべての変更を保存し、起動ライブラリを作成して、適切な環境に昇格します。

## リンクトラッキングビーコンとAnalytics呼び出しの検証

これで、 **CTA Clicked** ルールがAnalyticsビーコンを送信したので、Experience Platformデバッガーを使用してAnalyticsトラッキング変数を確認できるはずです。

1. ブラウザで [WKNDサイト](https://wknd.site/us/en.html) を開きます。
1. デバッガーアイコン「 ![Experience platform Debugger」アイコンをクリックし](assets/track-clicked-component/experience-cloud-debugger.png) 、Experience Platformデバッガーを開きます。
1. 前述のとおり、デバッガがLaunchプロパティを開発環境にマッピングし *ていることを確認し* 、[ **コンソールログ** ]がオンになっていることを確認します。
1. Analyticsメニューを開き、レポートスイートがレポートスイートに設定されている *ことを確認します* 。

   ![「解析」タブデバッガー](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザで、「Teaser **」ボタンまたは「** Button **** CTA」ボタンの1つをクリックして、別のページに移動します。

   ![CTAボタンをクリック](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platformデバッガーに戻り、下にスクロールして **Network Requests** / *Your Report Suiteを展開します*。 **eVar**、 **prop**、 **イベント** セットが見つかるはずです。

   ![クリック時に追跡されるAnalyticsイベント、eVarおよびprop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。 サイトのフッターに移動し、次のいずれかのナビゲーションリンクをクリックします。

   ![フッターの「ナビゲーション」リンクをクリックします。](assets/track-clicked-component/click-navigation-link-footer.png)

1. ブラウザーコンソールで、ルール「CTA Clicked」のメッセージ *「Custom Code」が満たされなかったことを確認します*。

   これは、Navigationコンポーネントは `cmp:click` イベントをトリガし *ますが* 、リソースタイプに対するチェックの結果、何も行われないためです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、 **Experience Platformデバッガの「** 起動 **」で「コンソールログ** 」がオンになっていることを確認してください。

## バリデーターが

先ほど、イベント主導型のAdobeクライアントデータレイヤーとExperience Platform Launchを使用して、Adobe Experience Managerサイトの特定のコンポーネントのクリックを追跡しました。