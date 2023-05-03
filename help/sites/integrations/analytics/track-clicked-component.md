---
title: クリックされたコンポーネントの Adobe Analytics での追跡
description: イベント駆動型の Adobe Client Data Layer を使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡します。タグルールを使用してこれらのイベントをリッスンし、トラックリンクビーコンを使用してAdobe Analyticsレポートスイートにデータを送信する方法について説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 43%

---

# クリックされたコンポーネントの Adobe Analytics での追跡

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 次を参照してください。 [文書](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 用語の変更を統合的に参照する場合。

イベント駆動型の [Adobe Client Data Layer を AEM コアコンポーネントと共に](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡します。タグプロパティでルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングし、トラックリンクビーコンと共にデータをAdobe Analyticsに送信する方法について説明します。

## 作成する内容 {#what-build}

WKND マーケティングチームは、どちらを知ることに関心があるかを知ることに興味を持っています `Call to Action (CTA)` ボタンがホームページで最も高いパフォーマンスを発揮します。 このチュートリアルでは、 `cmp:click` 次のイベント： **ティーザー** および **ボタン** コンポーネント。 次に、コンポーネント ID と新しいイベントをトラックリンクビーコンと共にAdobe Analyticsに送信します。

![クリックの追跡を作成する方法](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. イベントに基づくルールを、 `cmp:click` イベント。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. コンポーネント ID を設定し、トラッキングリンクビーコンと共にイベントをAdobe Analyticsに送信します。

## 前提条件

このチュートリアルは、[Adobe Analytics でページデータを収集](./collect-data-analytics.md)の続きであり、以下があることを前提としています。

* A **タグのプロパティ** と [Adobe Analytics拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ja) 有効
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバーがある。詳しくは、次のドキュメントを参照してください。 [レポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platformデバッガー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) タグプロパティをに読み込んで設定されたブラウザー拡張 [WKND サイト](https://wknd.site/us/en.html) またはAEMサイトで、Adobeデータレイヤーが有効になっている。

## Inspectボタンスキーマとティーザースキーマ

タグプロパティにルールを作成する前に、 [ボタンおよびティーザーのスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item) をクリックし、データレイヤー実装でそれらを調べます。

1. に移動します。 [WKND ホームページ](https://wknd.site/us/en.html)
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に移動します。次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   上記のコードは、クライアントデータレイヤーAdobeの現在の状態を返します。

   ![ブラウザーコンソールを使用したデータレイヤーの状態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 応答を展開し、`button-` および `teaser-xyz-cta` エントリで始まるエントリを見つけます。次のようなデータスキーマが表示されます。

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

   上記のデータの詳細は、 [コンポーネント/コンテナ項目スキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item). 新しいタグルールでは、このスキーマが使用されます。

## CTA クリック済みルールの作成

Adobe Client Data Layer は、**イベント**&#x200B;駆動型のデータレイヤーです。任意のコアコンポーネントがクリックされたときは常に、 `cmp:click` イベントは、データレイヤーを介してディスパッチされます。 をリッスンするには、以下を実行します。 `cmp:click` イベントで、ルールを作成します。

1. Experience Platformに移動し、AEM Site と統合されたタグプロパティに移動します。
1. 次に移動： **ルール** 」セクションをクリックし、 **ルールを追加**.
1. 「**CTA クリック済み**」という名前をルールに付けます。
1. **イベント**／**追加**&#x200B;をクリックして、**イベント設定**&#x200B;ウィザードを開きます。
1. の場合 **イベントタイプ** フィールド、選択 **カスタムコード**.

   ![「CTA クリック済み」という名前をルールに付け、カスタムコードイベントを追加](assets/track-clicked-component/custom-code-event.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   上記のコードスニペットで、データレイヤーに[関数をプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)して、イベントリスナーを追加します。常に `cmp:click` イベントがトリガーされた `componentClickedHandler` 関数が呼び出されます。 この関数では、いくつかのサニティチェックが追加され、新しい `event` オブジェクトは最新の [データレイヤーの状態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) イベントをトリガーしたコンポーネントの

   最後に `trigger(event)` 関数が呼び出されます。 この `trigger()` 関数は、タグプロパティ内の予約名で、 **トリガー** ルール。 この `event` オブジェクトはパラメーターとして渡され、その後、タグプロパティ内の別の予約名で公開されます。 タグプロパティ内のデータ要素は、 `event.component['someKey']`.

1. 変更を保存します。
1. 次に、**アクション**&#x200B;で「**追加**」をクリックして、**アクションの設定**&#x200B;ウィザードを開きます。
1. の場合 **アクションタイプ** フィールド、選択 **カスタムコード**.

   ![カスタムコードアクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event` オブジェクトは、カスタムイベントで呼び出された `trigger()` メソッドから渡されます。この `component` オブジェクトは、データレイヤーから派生したコンポーネントの現在の状態です `getState()` メソッドおよびは、クリックをトリガーした要素です。

1. 変更を保存し、 [ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) をタグプロパティに追加して、 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja) をAEM Site で使用している場合にのみ使用できます。

   >[!NOTE]
   >
   > これは、 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 埋め込みコードを **開発** 環境。

1. [WKND サイト](https://wknd.site/us/en.html)に移動し、デベロッパーツールを開いてコンソールを表示します。また、 **ログを保持** チェックボックス。

1. 「**ティーザー**」または「**ボタン**」のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![CTA クリックするボタン](assets/track-clicked-component/cta-button-to-click.png)

1. **CTA クリック済み**&#x200B;ルールが呼び出されたことを Developer Console で確認します。

   ![CTA ボタンクリック済み](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネント ID とタイトルを取り込みます。前の演習で確認したとおり、`event.path` の出力は `component.button-b6562c963d` に似たものであり、`event.component['dc:title']` の値は「View Trips」と同様のものでした。

### コンポーネント ID

1. Experience Platformに移動し、AEM Site と統合されたタグプロパティに移動します。
1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. の場合 **名前** フィールドに入力 **コンポーネント ID**.
1. の場合 **データ要素タイプ** フィールド、選択 **カスタムコード**.

   ![コンポーネント ID データ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. クリック **編集画面を開く** ボタンをクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 変更を保存します。

   >[!NOTE]
   >
   > 以下を思い出してください。 `event` オブジェクトは、 **ルール** タグプロパティ内。 データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。したがって、このデータ要素は、 **Page Loaded** 前の手順で作成されたルール *しかし* 他のコンテキストでは安全に使用できません。


### コンポーネントのタイトル

1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. の場合 **名前** フィールドに入力 **コンポーネントタイトル**.
1. の場合 **データ要素タイプ** フィールド、選択 **カスタムコード**.
1. クリック **編集画面を開く** ボタンをクリックし、カスタムコードエディターで次のように入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 変更を保存します。

## CTA クリック済みルールに条件を追加する

次に、 **CTA クリック済み** ルールを使用して、 `cmp:click` イベントが発生した場合、 **ティーザー** または **ボタン**. ティーザーの CTA はデータレイヤー内で個別のオブジェクトと見なされるので、親を調べて、ティーザーから来たことを確認することが重要です。

1. タグプロパティ UI で、 **CTA クリック済み** ルールが作成されました。
1. 「**条件**」で「**追加**」をクリックすると、**条件設定**&#x200B;ウィザードが表示されます。
1. の場合 **条件タイプ** フィールド、選択 **カスタムコード**.

   ![CTA クリック済み条件のカスタムコード](assets/track-clicked-component/custom-code-condition.png)

1. 「**編集画面を開く**」をクリックし、カスタムコードエディターに以下を入力します。

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

   上記のコードでは、最初に、リソースタイプが **ボタン** リソースタイプが **ティーザー**.

1. 変更を保存します。

## Analytics 変数の設定とリンクの追跡ビーコンのトリガー

現在、**CTA クリック済み**&#x200B;ルールは単にコンソールステートメントを出力します。次に、データ要素と Analytics 拡張機能を使用して、Analytics 変数を&#x200B;**アクション**&#x200B;として設定します。また、追加のアクションを設定して、 **リンクを追跡** 収集したデータをAdobe Analyticsに送信します。

1. 内 **CTA クリック済み** ルール **削除** の **コア — カスタムコード** アクション（コンソールステートメント）:

   ![「カスタムコードを削除」アクション](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」で、 **追加** をクリックしてアクションを作成します。
1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**変数を設定**&#x200B;に設定します。

1. 「**eVar**」、「**Prop**」および「**イベント**」に次の値を設定します。

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar、Prop およびイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここ `%Component ID%` は、クリックされた CTA の一意の識別子を保証するので、使用されます。 使用の潜在的な欠点 `%Component ID%` は、Analytics レポートに次のような値が含まれることを意味します。 `button-2e6d32893a`. の使用 `%Component Title%` によりわかりやすい名前が付けられますが、値が一意でない可能性があります。

1. 次に、「 **Adobe Analytics — 変数を設定** をタップすることで **プラス** アイコン：

   ![タグルールへの追加アクションの追加](assets/track-clicked-component/add-additional-launch-action.png)

1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**ビーコンを送信**&#x200B;に設定します。
1. 「**トラッキング**」で、ラジオボタンを **`s.tl()`** に設定します。
1. の場合 **リンクタイプ** フィールド、選択 **カスタムリンク** および **リンク名** 値を次の値に設定します。 **`%Component Title%: CTA Clicked`**:

   ![リンクビーコン送信の設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   上記の設定は、データ要素からの動的変数を組み合わせたものです **コンポーネントタイトル** と静的文字列 **CTA クリック済み**.

1. 変更を保存します。**CTA クリック済み**&#x200B;ルールの設定は、次のようになります。

   ![最終タグルール設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.**`cmp:click` イベントをリッスンします。
   * **2.** イベントが&#x200B;**ボタン**&#x200B;または&#x200B;**ティーザー**&#x200B;にトリガーされたことを確認します。
   * **3.** 追跡する Analytics 変数を設定 **コンポーネント ID** as a **eVar**, **prop**、および **イベント**.
   * **4.** Analytics のリンク追跡ビーコンを送信します（ページビューとして&#x200B;**扱わないようにします**）。

1. すべての変更を保存し、タグライブラリを構築して、適切な環境に昇格します。

## リンク追跡ビーコンと Analytics 呼び出しの検証

**CTA クリック済み**&#x200B;ルールで Analytics ビーコンを送信するようになったので、Experience Platform デバッガーを使用して Analytics トラッキング変数を確認できます。

1. ブラウザーで [WKND サイト](https://wknd.site/us/en.html)を開きます。
1. ![「Experience Platform Debugger」アイコン](assets/track-clicked-component/experience-cloud-debugger.png) の「デバッガー」アイコンをクリックして Experience Platform Debugger を起動します。
1. Debugger がタグプロパティをにマッピングしていることを確認します。 *あなたの* 前述のように開発環境、および **コンソールログ** がオンになっている。
1. Analytics メニューを開き、レポートスイートが&#x200B;*自分の*&#x200B;レポートスイートに設定されていることを確認します。

   ![デバッガーの「Analytics」タブ](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![クリックする CTA ボタン](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform デバッガーに戻り、下にスクロールして&#x200B;**ネットワークリクエスト**／*レポートスイート*&#x200B;を展開します。**eVar**、**prop** および&#x200B;**イベント**&#x200B;が設定されていることがわかります。

   ![クリック時に追跡される Analytics のイベント、eVar および prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。サイトのフッターに移動し、ナビゲーションリンクのいずれかをクリックします。

   ![フッターのナビゲーションリンクのクリック](assets/track-clicked-component/click-navigation-link-footer.png)

1. *ルール「CTA クリック済み」の「カスタムコード」が満たされませんでした*&#x200B;というメッセージがブラウザーコンソールに表示されます。

   上記のメッセージは、ナビゲーションコンポーネントがトリガー `cmp:click` イベント *しかし* 理由は [ルールの条件](#add-a-condition-to-the-cta-clicked-rule) がリソースの種類をチェックし、アクションは実行されません。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、 **コンソールログ** は以下でチェックされています **Experience Platformタグ** (Experience PlatformDebugger)

## おめでとうございます。

Experience Platformでイベント駆動型Adobeクライアントデータレイヤーとタグを使用して、AEMサイト上の特定のコンポーネントのクリックを追跡したのです。
