---
title: Adobe Analyticsでクリックされたコンポーネントを追跡
description: イベント主導型のAdobeクライアントデータレイヤーを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡します。 Experience Platform Launchのルールを使用して、これらのイベントをリッスンし、リンクトラッキングビーコンと共にデータをAdobe Analyticsに送信する方法について説明します。
feature: 分析
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
topic: 統合
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 5%

---


# Adobe Analyticsでクリックされたコンポーネントを追跡

イベント主導型の[AdobeクライアントデータレイヤーとAEMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/data-layer/overview.html)を使用して、Adobe Experience Managerサイトの特定のコンポーネントのクリックを追跡します。 Experience Platform Launch でルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作成する内容

WKNDマーケティングチームは、ホームページでどの誘い文句(CTA)ボタンが最も高いパフォーマンスを発揮しているかを把握したいと考えています。 このチュートリアルでは、**Teaser**&#x200B;および&#x200B;**Button**&#x200B;コンポーネントの`cmp:click`イベントをリッスンし、コンポーネントIDと新しいイベントをトラックリンクビーコンと共にAdobe Analyticsに送信する新しいルールをExperience Platform Launchに追加します。

![クリック追跡の対象](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. `cmp:click`イベントに基づいて、起動でイベント主導のルールを作成します。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. クリックされたコンポーネントIDを設定し、イベントAdobe Analyticsをリンクトラッキングビーコンと共に送信します。

## 前提条件

このチュートリアルは[Adobe Analytics](./collect-data-analytics.md)とのページデータ収集の続きで、次の点を想定しています。

* **[Adobe Analytics拡張子](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)が有効な起動プロパティ**
* **Adobe** 分析/開発レポートスイートIDとトラッキングサーバー。[新しいレポートスイート](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)の作成については、次のドキュメントを参照してください。
* [Experience Platformデ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) ータレイヤーが有効なAEMサイトのhttps://wknd.site/us/en. [](https://wknd.site/us/en.html) htmlまたはに読み込まれたLaunchプロパティを使用して設定されたAdobeデバッガーブラウザー拡張機能。

## Inspectザボタンアンドティーザースキーマ

「起動」でルールを作成する前に、ボタンとティーザー](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)の[スキーマを確認し、データレイヤー実装でそれらを調べると便利です。

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html)に移動します。
1. ブラウザーの開発者ツールを開き、**コンソール**&#x200B;に移動します。 次の コマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   Adobeクライアントデータレイヤーの現在の状態を返します。

   ![ブラウザーコンソールを使用したデータレイヤーの状態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 応答を展開し、`button-`と`teaser-xyz-cta`のエントリが先頭に付いたエントリを探します。 次のようなデータスキーマが表示されます。

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
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   これらは、[コンポーネント/コンテナ項目スキーマ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)に基づいています。 「開始」で作成するルールは、このスキーマを使用します。

## CTAクリック済みルールの作成

Adobe・クライアント・データ・レイヤーは、**イベント**&#x200B;駆動データ・レイヤーです。 いずれかのコアコンポーネントがクリックされると、`cmp:click`イベントがデータレイヤーを介してディスパッチされます。 次に、`cmp:click`イベントをリッスンするルールを作成します。

1. Experience Platform Launchに移動し、AEM Siteに統合されたWebプロパティに移動します。
1. UIの起動の&#x200B;**ルール**&#x200B;セクションに移動し、**ルール**&#x200B;追加をクリックします。
1. ルールに&#x200B;**CTA Clicked**&#x200B;という名前を付けます。
1. **イベント**/**追加**&#x200B;をクリックして、**イベントの設定**&#x200B;ウィザードを開きます。
1. 「**イベントタイプ**」で、「**カスタムコード**」を選択します。

   ![ルールに「CTA Clicked」という名前を付け、カスタムコードイベントを追加します。](assets/track-clicked-component/custom-code-event.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

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

   上記のコードスニペットでは、[関数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)をデータレイヤーにプッシュすることで、イベントリスナーを追加します。 `cmp:click`イベントがトリガされると、`componentClickedHandler`関数が呼び出されます。 この関数では、いくつかのサニティチェックが追加され、イベントをトリガーしたコンポーネントの最新の[状態のデータレイヤー](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)で新しい`event`オブジェクトが構築されます。

   その後`trigger(event)`が呼び出されます。 `trigger()` は、起動で予約された名前で、起動ルールが「トリガー」されます。`event`オブジェクトをパラメータとして渡し、その後、`event`という名前のLaunchで別の予約名で公開されます。 起動のデータ要素は、次のような様々なプロパティを参照できるようになりました。`event.component['someKey']`.

1. 変更内容を保存します。
1. **アクション**&#x200B;の次に&#x200B;**追加**&#x200B;をクリックして、**アクションの設定**&#x200B;ウィザードを開きます。
1. 「**アクションタイプ**」で、「**カスタムコード**」を選択します。

   ![カスタムコードアクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event`オブジェクトは、カスタムイベントで呼び出される`trigger()`メソッドから渡されます。 `component` は、クリックをトリガーしたデータレイヤーから派生したコンポーネント `getState` の現在の状態です。

1. 変更を保存し、起動に[build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)を実行して、コードをAEMサイトで使用されている[環境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)に昇格させます。

   >[!NOTE]
   >
   > [Adobe Experience Platformデバッガー](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)を使用して、埋め込みコードを&#x200B;**開発版**&#x200B;環境に切り替えると便利です。

1. [WKNDサイト](https://wknd.site/us/en.html)に移動し、開発者ツールを開いてコンソールを表示します。 「**ログを保存**」を選択します。

1. **ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のCTAボタンの1つをクリックして、別のページに移動します。

   ![CTAボタンをクリック](assets/track-clicked-component/cta-button-to-click.png)

1. 開発者コンソールで、**CTA Clicked**&#x200B;ルールが実行されたことを確認します。

   ![CTAボタンがクリックされました](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネントIDとタイトルを取り込みます。 前の練習で思い出してください。`event.path`の出力は`component.button-b6562c963d`に似ていて、`event.component['dc:title']`の値は&quot;表示トリップ&quot;のようなものでした。

### コンポーネントID

1. Experience Platform Launchに移動し、AEM Siteに統合されたWebプロパティに移動します。
1. 「**データ要素**」セクションに移動し、「**追加新しいデータ要素**」をクリックします。
1. **名前**&#x200B;に対して、**コンポーネントID**&#x200B;を入力します。
1. **データ要素タイプ**&#x200B;に対して、**カスタムコード**&#x200B;を選択します。

   ![コンポーネントIDデータ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. 「**エディターを開く**」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   変更内容を保存します。

   >[!NOTE]
   >
   > `event`オブジェクトが使用可能になり、起動で&#x200B;**ルール**&#x200B;をトリガーしたイベントに基づいて範囲が指定されることを忘れないでください。 データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。 したがって、このデータ要素は、ルール内で安全に使用できます。例えば、前の演習&#x200B;*で作成した&#x200B;**CTA Clicked**ルールのように、他のコンテキストでは使用できません。*

### コンポーネントのタイトル

1. 「**データ要素**」セクションに移動し、「**追加新しいデータ要素**」をクリックします。
1. **名前**&#x200B;に対して、**コンポーネントのタイトル**&#x200B;を入力します。
1. **データ要素タイプ**&#x200B;に対して、**カスタムコード**&#x200B;を選択します。
1. 「**エディターを開く**」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   変更内容を保存します。

## CTA追加クリックルールの条件

次に、**CTA Clicked**&#x200B;ルールを更新し、**Teaser**&#x200B;または&#x200B;**Button**&#x200B;で`cmp:click`イベントが起動したときにのみルールが起動されるようにします。 TeaserのCTAはデータレイヤー内の別のオブジェクトと見なされるので、親をチェックしてTeaserから来ているかどうかを確認することが重要です。

1. 起動UIで、前に作成した&#x200B;**CTA Clicked**&#x200B;ルールに移動します。
1. 「**条件**」で「**追加**」をクリックし、**条件の設定**&#x200B;ウィザードを開きます。
1. 「**条件の種類**」には、「**カスタムコード**」を選択します。

   ![CTAクリック条件カスタムコード](assets/track-clicked-component/custom-code-condition.png)

1. 「**エディターを開く**」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   上記のコードは、最初にリソースタイプが&#x200B;**Button**&#x200B;からのものかどうかを調べ、次に&#x200B;**Teaser**&#x200B;内のCTAからのものかどうかを調べます。

1. 変更内容を保存します。

## Analytics変数とトリガー追跡リンクビーコンの設定

現在、**CTA Clicked**&#x200B;ルールは、単にコンソールステートメントを出力します。 次に、データ要素とAnalytics拡張機能を使用して、Analytics変数を&#x200B;**アクション**&#x200B;として設定します。 また、**リンクを追跡**&#x200B;をトリガーするための追加のアクションを設定し、収集したデータをAdobe Analyticsに送信します。

1. **CTAクリック**&#x200B;ルール&#x200B;**削除****コア — カスタムコード**&#x200B;アクション（コンソール文）内：

   ![カスタムコードアクションの削除](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」の下の&#x200B;**追加**&#x200B;をクリックして、新しいアクションを追加します。
1. **拡張子**&#x200B;の種類を&#x200B;**Adobe Analytics**&#x200B;に設定し、**アクションタイプ**&#x200B;を&#x200B;**変数を設定**&#x200B;に設定します。

1. **eVars**、**Props**、**イベント**&#x200B;に次の値を設定します。

   * `evar8` -  `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![eVarpropとイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここ`%Component ID%`は、クリックされたCTAの一意の識別子を取得するので使用されます。 `%Component ID%`を使用すると、`button-2e6d32893a`のような値がAnalyticsレポートに含まれる可能性が低くなります。 `%Component Title%`を使用すると、わかりやすい名前が付けられますが、値が一意でない可能性があります。

1. 次に、**Adobe Analyticsの右側に「アクション」を追加し、**&#x200B;プラス&#x200B;**アイコンをタップして、「変数を設定**」を設定します。

   ![追加追加の開始アクション](assets/track-clicked-component/add-additional-launch-action.png)

1. **拡張機能**&#x200B;の種類を&#x200B;**Adobe Analytics**&#x200B;に設定し、**アクションの種類**&#x200B;を&#x200B;**ビーコンを送信**&#x200B;に設定します。
1. 「**追跡**」で、ラジオボタンを&#x200B;**`s.tl()`**&#x200B;に設定します。
1. **リンクタイプ**&#x200B;は「**カスタムリンク**」を選択し、**リンク名**&#x200B;は次の値に設定します。**`%Component Title%: CTA Clicked`**:

   ![リンクビーコンの送信の設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   これにより、データ要素&#x200B;**コンポーネントのタイトル**&#x200B;の動的変数と、静的文字列&#x200B;**CTA Clicked**&#x200B;の動的変数が組み合わされます。

1. 変更内容を保存します。**CTA Clicked**&#x200B;ルールの設定は次のようになります。

   ![最終起動の設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.**  `cmp:click` イベントをリッスンします。
   * **2.** イベントが **** Buttonor  **Teaserによってトリガーされたことを確認します**。
   * **3.** コンポー **ネントIDを** eVar **、** prop **、** ****&#x200B;イベントとして追跡するように、Analytics変数をに設定します。
   * **4.** Analyticsリンク追跡ビーコンを送信します(ページ表示として扱わ **** ない)。

1. すべての変更を保存し、起動ライブラリを作成して、適切な環境に昇格します。

## リンクトラッキングビーコンとAnalytics呼び出しの検証

これで、**CTA Clicked**&#x200B;ルールがAnalyticsビーコンを送信したので、Experience Platformデバッガーを使用してAnalyticsトラッキング変数を表示できるはずです。

1. ブラウザーで[WKNDサイト](https://wknd.site/us/en.html)を開きます。
1. デバッガーアイコン![エクスペリエンスプラットフォームデバッガーアイコン](assets/track-clicked-component/experience-cloud-debugger.png)をクリックして、Experience Platformデバッガーを開きます。
1. 前述のように、デバッガーがLaunchプロパティを&#x200B;**&#x200B;開発環境にマッピングし、**コンソールログ**&#x200B;がオンになっていることを確認します。
1. Analyticsメニューを開き、レポートスイートが&#x200B;**&#x200B;レポートスイートに設定されていることを確認します。

   ![「解析」タブデバッガー](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のCTAボタンの1つをクリックして、別のページに移動します。

   ![CTAボタンをクリック](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platformデバッガーに戻り、下にスクロールして&#x200B;**ネットワークリクエスト** > *レポートスイート*&#x200B;を展開します。 **eVar**、**prop**、**イベント**&#x200B;セットを見つけることができます。

   ![クリック時に追跡されるAnalyticsイベント、eVarおよびprop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。 サイトのフッターに移動し、次のいずれかのナビゲーションリンクをクリックします。

   ![フッターの「ナビゲーション」リンクをクリックします。](assets/track-clicked-component/click-navigation-link-footer.png)

1. ブラウザーコンソールで、ルール「CTA Clicked」のメッセージ&#x200B;*「Custom Code」が満たされなかったことを確認します。*

   これは、Navigationコンポーネントが`cmp:click`イベント&#x200B;*をトリガーし、*&#x200B;を行うのは、リソースタイプに対するチェックの結果、何も行われないからです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、Experience Platformデバッガーの&#x200B;**Launch**&#x200B;で&#x200B;**Console Logging**&#x200B;がチェック済みであることを確認してください。

## バリデーターが

先ほど、イベント主導型のAdobeクライアントデータレイヤーとExperience Platform Launchを使用して、Adobe Experience Managerサイトの特定のコンポーネントのクリックを追跡しました。