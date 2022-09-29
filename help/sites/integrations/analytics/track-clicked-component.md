---
title: Adobe Analyticsを使用したクリックされたコンポーネントの追跡
description: イベントドリブン型Adobeクライアントデータレイヤーを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡します。 Experience Platform Launchでルールを使用してこれらのイベントをリッスンし、トラックリンクビーコンと共にAdobe Analyticsにデータを送信する方法について説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 4%

---

# Adobe Analyticsを使用したクリックされたコンポーネントの追跡

イベント駆動型の使用 [AdobeクライアントデータレイヤーとAEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja) Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡する場合。 Experience Platform Launch でルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作成する内容

WKND マーケティングチームは、どのコールトゥアクション (CTA) ボタンがホームページで最も高いパフォーマンスを発揮しているかを把握したいと考えています。 このチュートリアルでは、をリッスンする新しいルールをExperience Platform Launchに追加します。 `cmp:click` 次のイベント： **ティーザー** および **ボタン** コンポーネントをクリックし、トラックリンクビーコンと共にコンポーネント ID と新しいイベントをAdobe Analyticsに送信します。

![クリックの追跡を作成する内容](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. Launch で、 `cmp:click` イベント。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. クリックしたコンポーネント ID を設定し、トラッキングリンクビーコンでイベントAdobe Analyticsを送信します。

## 前提条件

このチュートリアルは、 [Adobe Analyticsでのページデータの収集](./collect-data-analytics.md) およびは、以下があることを前提としています。

* A **Launch プロパティ** と [Adobe Analytics拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html?lang=ja) 有効
* **Adobe Analytics** テスト/開発レポートスイート ID とトラッキングサーバー。 詳しくは、次のドキュメントを参照してください。 [新しいレポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platformデバッガー](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Launch プロパティがに読み込まれた状態で設定されたブラウザー拡張機能 [https://wknd.site/us/en.html](https://wknd.site/us/en.html) またはAEMサイトで、Adobeデータレイヤーが有効になっている。

## Inspectボタンとティーザースキーマ

Launch でルールを作成する前に、 [ボタンおよびティーザーのスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) をクリックし、データレイヤー実装でそれらを調べます。

1. に移動します。 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. ブラウザーの開発者ツールを開き、 **コンソール**. 次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   これは、クライアントデータレイヤーのAdobeの状態を返します。

   ![ブラウザーコンソールを使用したデータレイヤーの状態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 応答を展開し、というプレフィックスが付いたエントリを見つけます。 `button-` および  `teaser-xyz-cta` エントリ。 次のようなデータスキーマが表示されます。

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

   これらは、 [コンポーネント/コンテナ項目スキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). Launch で作成するルールは、このスキーマを使用します。

## CTA クリック済みルールの作成

Adobeクライアントデータレイヤーは **イベント** 駆動型データレイヤー。 任意のコアコンポーネントが `cmp:click` イベントは、データレイヤーを介してディスパッチされます。 次に、 `cmp:click` イベント。

1. Experience Platform Launchに移動し、AEM Site と統合された Web プロパティに移動します。
1. 次に移動： **ルール** Launch UI のセクションでセクションを開き、 **ルールを追加**.
1. ルールに名前を付ける **CTA クリック済み**.
1. クリック **イベント** > **追加** 開く **イベント設定** ウィザード。
1. の下 **イベントタイプ** 選択 **カスタムコード**.

   ![ルールに「CTA クリック済み」という名前を付け、カスタムコードイベントを追加します。](assets/track-clicked-component/custom-code-event.png)

1. クリック **編集画面を開く** メインパネルで、次のコードスニペットを入力します。

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

   上記のコードスニペットは、 [関数のプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) をデータレイヤーに追加します。 次の場合に `cmp:click` イベントがトリガーされた `componentClickedHandler` 関数が呼び出されます。 この関数では、いくつかのサニティチェックが追加され、新しい `event` オブジェクトは最新の [データレイヤーの状態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) イベントをトリガーしたコンポーネントの

   その後 `trigger(event)` が呼び出されます。 `trigger()` は、Launch で予約されている名前で、Launch ルールの「トリガー」です。 我々は、 `event` オブジェクトは、次に、Launch の別の予約名で公開され、 `event`. Launch のデータ要素で、次のような様々なプロパティを参照できるようになりました。 `event.component['someKey']`.

1. 変更を保存します。
1. 次の下 **アクション** クリック **追加** 開く **アクションの設定** ウィザード。
1. の下 **アクションタイプ** 選択 **カスタムコード**.

   ![「Custom Code」アクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. クリック **編集画面を開く** メインパネルで、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   この `event` オブジェクトが `trigger()` メソッドがカスタムイベントで呼び出されました。 `component` は、データレイヤーから派生したコンポーネントの現在の状態です `getState` それがクリックを引き起こした。

1. 変更を保存し、 [ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) （Launch で）をクリックして、 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) をAEM Site で使用している場合にのみ使用できます。

   >[!NOTE]
   >
   > これは、 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) 埋め込みコードを **開発** 環境。

1. 次に移動： [WKND サイト](https://wknd.site/us/en.html) 開発者ツールを開いてコンソールを表示します。 選択 **ログを保持**.

1. いずれかの **ティーザー** または **ボタン** 別のページに移動するための CTA ボタン。

   ![クリックする CTA ボタン](assets/track-clicked-component/cta-button-to-click.png)

1. デベロッパーコンソールで、 **CTA クリック済み** ルールが実行されました：

   ![CTA ボタンのクリック](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネント ID とタイトルを取り込みます。 前の練習でのの出力の再現 `event.path` は～に似ていた `component.button-b6562c963d` そして、 `event.component['dc:title']` は「旅行を見る」のようなものでした。

### コンポーネント ID

1. Experience Platform Launchに移動し、AEM Site と統合された Web プロパティに移動します。
1. 次に移動： **データ要素** 「 」セクションで、「 」をクリックします。 **新規データ要素の追加**.
1. の場合 **名前** 入力 **コンポーネント ID**.
1. の場合 **データ要素タイプ** 選択 **カスタムコード**.

   ![コンポーネント ID データ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. クリック **編集画面を開く** カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   変更を保存します。

   >[!NOTE]
   >
   > 以下を思い出してください。 `event` オブジェクトは、 **ルール** Launch 内。 データ要素の値は、データ要素が *参照* ルール内で使用できます。 したがって、このデータ要素は、 **CTA クリック済み** 前の演習で作成したルール *しかし* 他のコンテキストでは安全に使用できません。

### コンポーネントのタイトル

1. 次に移動： **データ要素** 「 」セクションで、「 」をクリックします。 **新規データ要素の追加**.
1. の場合 **名前** 入力 **コンポーネントタイトル**.
1. の場合 **データ要素タイプ** 選択 **カスタムコード**.
1. クリック **編集画面を開く** カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   変更を保存します。

## CTA クリック済みルールに条件を追加する

次に、 **CTA クリック済み** ルールを設定し、 `cmp:click` イベントが発生した場合、 **ティーザー** または **ボタン**. ティーザーの CTA はデータレイヤー内で別のオブジェクトと見なされるので、親を調べて、ティーザーから来たことを確認することが重要です。

1. Launch UI で、 **CTA クリック済み** ルールが作成されました。
1. の下 **条件** クリック **追加** 開く **条件の設定** ウィザード。
1. の場合 **条件タイプ** 選択 **カスタムコード**.

   ![CTA クリック条件のカスタムコード](assets/track-clicked-component/custom-code-condition.png)

1. クリック **編集画面を開く** カスタムコードエディターで次のように入力します。

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

   上記のコードでは、最初に、リソースタイプが **ボタン** 次に、リソースタイプが CTA からのものか、 **ティーザー**.

1. 変更を保存します。

## Analytics 変数とトリガー追跡リンクビーコンの設定

現在、 **CTA クリック済み** ルールは、単にコンソールステートメントを出力します。 次に、データ要素と Analytics 拡張機能を使用して、Analytics 変数を **アクション**. また、をトリガーする追加のアクションを設定します **リンクを追跡** 収集したデータをAdobe Analyticsに送信します。

1. 内 **CTA クリック済み** ルール **削除** の **コア — カスタムコード** アクション（コンソールステートメント）:

   ![「Remove custom code」アクション](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」で、 **追加** をクリックして新しいアクションを追加します。
1. を **拡張** 入力 **Adobe Analytics** そして、 **アクションタイプ** から  **変数を設定**.

1. 次の値を **eVar**, **Props**、および **イベント**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVarProp とイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここ `%Component ID%` は、クリックされた CTA の一意の識別子を取得するので使用されます。 使用の潜在的な欠点 `%Component ID%` は、Analytics レポートに次のような値が含まれることを示します。 `button-2e6d32893a`. 使用 `%Component Title%` によりわかりやすい名前が付けられますが、値が一意でない可能性があります。

1. 次に、の右側に「アクション」を追加します。 **Adobe Analytics — 変数を設定** をタップすることで **プラス** アイコン：

   ![Launch アクションの追加](assets/track-clicked-component/add-additional-launch-action.png)

1. を **拡張** 入力 **Adobe Analytics** そして、 **アクションタイプ** から  **ビーコンを送信**.
1. の下 **トラッキング** ラジオボタンを **`s.tl()`**.
1. の場合 **リンクタイプ** 選択 **カスタムリンク** および **リンク名** 値を次の値に設定します。 **`%Component Title%: CTA Clicked`**:

   ![リンク送信ビーコンの設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   これにより、データ要素の動的変数が組み合わされます **コンポーネントタイトル** と静的文字列 **CTA クリック済み**.

1. 変更内容を保存します。この **CTA クリック済み** ルールは、次の設定になるはずです。

   ![最終起動設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** をリッスンします。 `cmp:click` イベント。
   * **2.** イベントが **ボタン** または **ティーザー**.
   * **3.** を追跡するためにの Analytics 変数を設定 **コンポーネント ID** as a **eVar**, **prop**、および **イベント**.
   * **4.** Analytics トラッキングリンクビーコンを送信する（および実行する） **not** ページビューとして扱います )。

1. すべての変更を保存し、Launch ライブラリを構築して、適切な環境に昇格します。

## トラッキングリンクビーコンと Analytics 呼び出しの検証

これで、 **CTA クリック済み** ルールが Analytics ビーコンを送信する場合は、Analytics Debugger を使用して Analytics トラッキング変数をExperience Platformできます。

1. を開きます。 [WKND サイト](https://wknd.site/us/en.html) ブラウザーに表示されます。
1. デバッガーアイコンをクリックします。 ![Experience Platform Debugger アイコン](assets/track-clicked-component/experience-cloud-debugger.png) をクリックして、Experience PlatformDebugger を開きます。
1. デバッガーが Launch プロパティをにマッピングしていることを確認します。 *あなたの* 開発環境（前述のとおり） **コンソールログ** がオンになっている。
1. Analytics メニューを開き、レポートスイートが *あなたの* レポートスイートを使用します。

   ![「Analytics」タブデバッガー](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、 **ティーザー** または **ボタン** 別のページに移動するための CTA ボタン。

   ![クリックする CTA ボタン](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platformデバッガーに戻り、下にスクロールして展開します。 **ネットワークリクエスト** > *レポートスイート*. 次を見つけることができます： **eVar**, **prop**、および **イベント** 設定します。

   ![クリック時に追跡される Analytics のイベント、eVar および prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。 サイトのフッターに移動し、次のいずれかのナビゲーションリンクをクリックします。

   ![フッターのナビゲーションリンクをクリックします。](assets/track-clicked-component/click-navigation-link-footer.png)

1. ブラウザーコンソールで、メッセージを確認します。 *ルール「CTA のクリック」の「カスタムコード」が満たされなかった問題を修正しました*.

   これは、ナビゲーションコンポーネントがトリガーa `cmp:click` イベント *しかし* リソースタイプに対するの確認のため、アクションは実行されません。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、 **コンソールログ** は以下でチェックされています **起動** (Experience PlatformDebugger)

## おめでとうございます。

イベント駆動型AdobeクライアントデータレイヤーとExperience Platform Launchを使用して、Adobe Experience Managerサイト上の特定のコンポーネントのクリックを追跡しました。
