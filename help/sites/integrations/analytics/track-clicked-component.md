---
title: Adobe Analyticsを使用したクリックされたコンポーネントの追跡
description: イベントドリブン型Adobeクライアントデータレイヤーを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡します。 Experience Platform Launchでルールを使用してこれらのイベントをリッスンし、トラックリンクビーコンと共にAdobe Analyticsにデータを送信する方法を説明します。
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 5%

---


# Adobe Analyticsを使用したクリックされたコンポーネントの追跡

イベント駆動型の[AdobeクライアントデータレイヤーとAEMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/data-layer/overview.html)を使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡します。 Experience Platform Launch でルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作成する内容

WKNDマーケティングチームは、どのコールトゥアクション(CTA)ボタンがホームページで最も高いパフォーマンスを発揮しているかを把握したいと考えています。 このチュートリアルでは、**Teaser**&#x200B;および&#x200B;**Button**&#x200B;コンポーネントの`cmp:click`イベントをリッスンし、コンポーネントIDと新しいイベントをトラックリンクビーコンと共にAdobe Analyticsに送信する新しいルールをExperience Platform Launchに追加します。

![作成する内容によるクリックの追跡](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. Launchで、 `cmp:click`イベントに基づいてイベント駆動型ルールを作成します。
1. コンポーネントリソースタイプで様々なイベントをフィルターします。
1. クリックしたコンポーネントIDを設定し、追跡リンクビーコンを使用してイベントAdobe Analyticsを送信します。

## 前提条件

このチュートリアルは、[Adobe Analytics](./collect-data-analytics.md)を使用してページデータを収集するのと同じです。以下の点が前提となっています。

* [Adobe Analytics拡張機能](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)が有効な&#x200B;**Launchプロパティ**
* **Adobe** Analyticstest/devレポートスイートIDとトラッキングサーバー。[新しいレポートスイート](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)の作成については、次のドキュメントを参照してください。
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) データレイヤーが有効なAEMサイトに読み込まれたLaunchプロパ [ティを使用して設定されたAdobeデバッガーブラウザー拡張機能。https://wknd.site/us/en](https://wknd.site/us/en.html) 

## Inspect the ButtonおよびTeaser Schema

Launchでルールを作成する前に、ボタンとティーザー](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)の[スキーマを確認し、データレイヤー実装でそれらを調べると役に立ちます。

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html)に移動します。
1. ブラウザーの開発者ツールを開き、**コンソール**&#x200B;に移動します。 次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   これは、クライアントデータレイヤーのAdobeの現在の状態を返します。

   ![ブラウザーコンソールを使用したデータレイヤーの状態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 応答を展開し、`button-`と`teaser-xyz-cta`のプレフィックスが付いたエントリを見つけます。 次のようなデータスキーマが表示されます。

   ボタンスキーマ：

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   ティーザースキーマ：

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   これらは、[コンポーネント/コンテナ項目スキーマ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)に基づいています。 Launchで作成するルールでは、このスキーマを使用します。

## CTAクリックルールの作成

Adobeクライアントデータレイヤーは、**イベント**&#x200B;駆動型データレイヤーです。 任意のコアコンポーネントがクリックされると、データレイヤーを介して`cmp:click`イベントがディスパッチされます。 次に、`cmp:click`イベントをリッスンするルールを作成します。

1. Experience Platform Launchに移動し、AEM Siteと統合されたWebプロパティに移動します。
1. Launch UIの「**ルール**」セクションに移動し、「**ルールを追加**」をクリックします。
1. ルールに「**CTA Clicked**」と名前を付けます。
1. **イベント** / **追加**&#x200B;をクリックして、**イベント設定**&#x200B;ウィザードを開きます。
1. 「**イベントタイプ**」で、「**カスタムコード**」を選択します。

   ![ルールに「CTAクリック済み」という名前を付け、カスタムコードイベントを追加します。](assets/track-clicked-component/custom-code-event.png)

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

   上記のコードスニペットでは、[関数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)をデータレイヤーにプッシュしてイベントリスナーを追加します。 `cmp:click`イベントがトリガーされると、`componentClickedHandler`関数が呼び出されます。 この関数では、いくつかのサニティチェックが追加され、イベントをトリガーしたコンポーネントのデータレイヤー](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)の最新の[状態で新しい`event`オブジェクトが構築されます。

   その後、`trigger(event)`が呼び出されます。 `trigger()` は、Launchでの予約名で、Launchルールを「トリガー」します。`event`オブジェクトをパラメーターとして渡し、その後、`event`という名前のLaunchで別の予約名で公開します。 Launchのデータ要素で、次のような様々なプロパティを参照できるようになりました。`event.component['someKey']`.

1. 変更内容を保存します。
1. 次に、「**アクション**」の下の「**追加**」をクリックして、「**アクションの設定**」ウィザードを開きます。
1. 「**アクションタイプ**」で、「**カスタムコード**」を選択します。

   ![「Custom Code」アクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event`オブジェクトは、カスタムイベントで呼び出される`trigger()`メソッドから渡されます。 `component` は、クリックをトリガーしたデータレイヤーから派生したコンポ `getState` ーネントの現在の状態です。

1. 変更を保存し、Launchで[ビルド](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)を実行して、AEMサイトで使用する[環境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)にコードを昇格させます。

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)を使用して、埋め込みコードを&#x200B;**開発**&#x200B;環境に切り替えると非常に役立ちます。

1. [WKND Site](https://wknd.site/us/en.html)に移動し、開発者ツールを開いてコンソールを表示します。 **ログを保存**&#x200B;を選択します。

1. **ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のCTAボタンの1つをクリックして、別のページに移動します。

   ![クリックするCTAボタン](assets/track-clicked-component/cta-button-to-click.png)

1. 開発者コンソールで、**CTA Clicked**&#x200B;ルールが実行されたことを確認します。

   ![CTAボタンがクリックされました](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネントIDとタイトルを取り込みます。 前の練習では、`event.path`の出力は`component.button-b6562c963d`に似ており、`event.component['dc:title']`の値は「View Trips」のようなものです。

### コンポーネントID

1. Experience Platform Launchに移動し、AEM Siteと統合されたWebプロパティに移動します。
1. 「**データ要素**」セクションに移動し、「**新しいデータ要素を追加**」をクリックします。
1. **名前**&#x200B;には、**コンポーネントID**&#x200B;と入力します。
1. **データ要素のタイプ**&#x200B;に対して、**カスタムコード**&#x200B;を選択します。

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
   > `event`オブジェクトは、Launchで&#x200B;**ルール**&#x200B;をトリガーしたイベントに基づいて使用可能になり、範囲が指定されます。 データ要素の値は、ルール内でデータ要素が&#x200B;*参照*&#x200B;されるまで設定されません。 したがって、このデータ要素は、前の演習&#x200B;*で作成した&#x200B;**CTA Clicked**ルールのようなルール内で安全に使用できますが、*&#x200B;は他のコンテキストでは安全に使用できません。

### コンポーネントのタイトル

1. 「**データ要素**」セクションに移動し、「**新しいデータ要素を追加**」をクリックします。
1. **名前**&#x200B;には、**コンポーネントのタイトル**&#x200B;と入力します。
1. **データ要素のタイプ**&#x200B;に対して、**カスタムコード**&#x200B;を選択します。
1. 「**エディターを開く**」をクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   変更内容を保存します。

## CTAクリックルールへの条件の追加

次に、**CTA Clicked**&#x200B;ルールを更新し、**Teaser**&#x200B;または&#x200B;**Button**&#x200B;に対して`cmp:click`イベントが発生した場合にのみルールが実行されるようにします。 ティーザーのCTAはデータレイヤー内で別のオブジェクトと見なされるので、親を調べてティーザーから来たことを確認することが重要です。

1. Launch UIで、前に作成した&#x200B;**CTAクリック**&#x200B;ルールに移動します。
1. 「**条件**」で「**追加**」をクリックし、「**条件の設定**」ウィザードを開きます。
1. 「**条件の種類**」で、「**カスタムコード**」を選択します。

   ![CTAクリック条件のカスタムコード](assets/track-clicked-component/custom-code-condition.png)

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

   上記のコードは、最初にリソースタイプが&#x200B;**ボタン**&#x200B;からのものかどうかを確認し、次に、**ティーザー**&#x200B;内のCTAからのものかどうかを確認します。

1. 変更内容を保存します。

## Analytics変数とトリガー追跡リンクビーコンの設定

現在、**CTA Clicked**&#x200B;ルールは、単にコンソールステートメントを出力します。 次に、データ要素とAnalytics拡張機能を使用して、Analytics変数を&#x200B;**アクション**&#x200B;として設定します。 また、**リンクを追跡**&#x200B;をトリガーし、収集したデータをAdobe Analyticsに送信する追加のアクションも設定します。

1. **CTAのクリック**&#x200B;ルール&#x200B;****&#x200B;を削除&#x200B;**コア — カスタムコード**&#x200B;アクション（コンソールステートメント）:

   ![「Remove custom code」アクション](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」の下の「**追加**」をクリックして、新しいアクションを追加します。
1. **拡張機能**&#x200B;タイプを&#x200B;**Adobe Analytics**&#x200B;に設定し、**アクションタイプ**&#x200B;を&#x200B;**変数**&#x200B;に設定します。

1. **eVars**、**Props**&#x200B;および&#x200B;**Events**&#x200B;に次の値を設定します。

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![eVarpropとイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここで`%Component ID%`は、クリックされたCTAの一意の識別子を取得するので、使用されます。 `%Component ID%`を使用する場合の潜在的な欠点は、Analyticsレポートに`button-2e6d32893a`のような値が含まれることです。 `%Component Title%`を使用すると、わかりやすい名前が付けられますが、値が一意でない可能性があります。

1. 次に、**プラス**&#x200B;アイコンをタップして、**Adobe Analyticsの右側に「アクション」を追加し、変数**&#x200B;を設定します。

   ![Launchアクションの追加](assets/track-clicked-component/add-additional-launch-action.png)

1. **Extension**&#x200B;タイプを&#x200B;**Adobe Analytics**&#x200B;に設定し、**アクションタイプ**&#x200B;を&#x200B;**Send Beacon**&#x200B;に設定します。
1. 「**トラッキング**」で、ラジオボタンを&#x200B;**`s.tl()`**&#x200B;に設定します。
1. **Link Type**&#x200B;には&#x200B;**Custom Link**&#x200B;を選択し、**Link Name**&#x200B;には次の値を設定します。**`%Component Title%: CTA Clicked`**:

   ![リンク送信ビーコンの設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   これにより、データ要素&#x200B;**コンポーネントのタイトル**&#x200B;と静的文字列&#x200B;**CTA Clicked**&#x200B;の動的変数が組み合わされます。

1. 変更内容を保存します。**CTA Clicked**&#x200B;ルールには、次の設定が含まれる必要があります。

   ![最終起動設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** イベントをリッスン `cmp:click` します。
   * **2.** イベントがボタンまたはティーザーによってトリガーされ **** たことを **確認します**。
   * **3.** コンポーネントIDを **eVar** 、 **prop**&#x200B;およびイベン **ト**&#x200B;として追跡するには、のAnalytics変数を設 ****&#x200B;定します。
   * **4.** Analyticsトラッキングリンクビーコンを送信します( **** ページビューとしては扱いません)。

1. すべての変更を保存し、Launchライブラリをビルドして、適切な環境に昇格します。

## トラッキングリンクビーコンとAnalytics呼び出しの検証

これで、**CTA Clicked**&#x200B;ルールがAnalyticsビーコンを送信したので、Experience Platformデバッガーを使用してAnalyticsトラッキング変数を表示できます。

1. ブラウザーで[WKND Site](https://wknd.site/us/en.html)を開きます。
1. デバッガーアイコン![Experience platform Debuggerアイコン](assets/track-clicked-component/experience-cloud-debugger.png)をクリックして、Experience Platformデバッガーを開きます。
1. 前述のように、デバッガーがLaunchプロパティを&#x200B;*お使いの*&#x200B;開発環境にマッピングしていることを確認し、**コンソールログ**&#x200B;を確認します。
1. Analyticsメニューを開き、レポートスイートが&#x200B;**&#x200B;レポートスイートに設定されていることを確認します。

   ![Analyticsタブデバッガー](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、「**ティーザー**」または「**ボタン**」CTAボタンの1つをクリックして、別のページに移動します。

   ![クリックするCTAボタン](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platformデバッガーに戻り、下にスクロールして&#x200B;**ネットワークリクエスト** / *レポートスイート*&#x200B;を展開します。 **eVar**、**prop**、**イベント**&#x200B;セットが見つかります。

   ![クリック時に追跡されるAnalyticsイベント、eVarおよびprop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。 サイトのフッターに移動し、次のいずれかのナビゲーションリンクをクリックします。

   ![フッターの「ナビゲーション」リンクをクリックします。](assets/track-clicked-component/click-navigation-link-footer.png)

1. ブラウザーコンソールで、ルール「CTAクリック」のメッセージ「*」「カスタムコード」が満たされなかったことを確認します。*

   これは、ナビゲーションコンポーネントが`cmp:click`イベント&#x200B;*をトリガーし、*&#x200B;をチェックしているので、アクションが実行されないためです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、Experience Platformデバッガーの&#x200B;**Launch**&#x200B;で&#x200B;**コンソールログ**&#x200B;がオンになっていることを確認してください。

## バリデーターが

イベントドリブン型のAdobeクライアントデータレイヤーとExperience Platform Launchを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡しただけです。