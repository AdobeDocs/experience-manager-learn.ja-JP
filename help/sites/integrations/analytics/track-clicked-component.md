---
title: クリックされたコンポーネントの Adobe Analytics での追跡
description: イベント駆動型の Adobe Client Data Layer を使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡します。Experience Platform Launch でルールを使用してクリックイベントをリッスンし、リンクの追跡ビーコンと共にデータを Adobe Analytics に送信する方法について説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '1806'
ht-degree: 100%

---

# クリックされたコンポーネントの Adobe Analytics での追跡

イベント駆動型の [Adobe Client Data Layer を AEM コアコンポーネントと共に](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡します。Experience Platform Launch でルールを使用して、クリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作成する内容

WKND マーケティングチームは、どの「コールトゥアクション（CTA）」ボタンがホームページで最も高いパフォーマンスを発揮しているかを把握したいと考えています。このチュートリアルでは、Experience Platform Launch で、**ティーザー**&#x200B;コンポーネントと&#x200B;**ボタン**&#x200B;コンポーネントから `cmp:click` イベントをリッスンする新しいルールし、リンクの追跡ビーコンと共にコンポーネント ID と新しいイベントを Adobe Analytics に送信します。

![クリックの追跡を作成する方法](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. `cmp:click` イベントに基づいて、Launch でイベント駆動型のルールを作成します。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. クリックされたコンポーネント ID を設定し、リンクの追跡ビーコンとイベントを Adobe Analytics に送信します。

## 前提条件

このチュートリアルは、[Adobe Analytics でページデータを収集](./collect-data-analytics.md)の続きであり、以下があることを前提としています。

* [Adobe Analytics 拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html?lang=ja)が有効になっている **Launch プロパティ**
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバー。[新しいレポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html?lang=ja)については、次のドキュメントを参照してください。
* [https://wknd.site/us/en.html](https://wknd.site/us/en.html) またはアドビのデータレイヤーが有効になっている AEM サイトに読み込まれた、Launch プロパティを使用して設定された [Experience Platform デバッガー](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html?lang=ja)ブラウザー拡張機能。

## ボタンとティーザーのスキーマの検査

Launch でルールを作成する前に、[ボタンとティーザーのスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item)を確認して、データレイヤーの実装で検査します。

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html) に移動します。
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に移動します。次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   これは、Adobe Client Data Layer の現在の状態を返します。

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

   これらは、[コンポーネント／コンテナ項目スキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item)に基づいています。Launch で作成するルールには、このスキーマを使用します。

## CTA クリック済みルールの作成

Adobe Client Data Layer は、**イベント**&#x200B;駆動型のデータレイヤーです。任意のコアコンポーネントがクリックされると、データレイヤーを介して `cmp:click` イベントがディスパッチされます。次に、`cmp:click` イベントをリッスンするルールを作成します。

1. Experience Platform Launch に移動し、AEM サイトと統合された web プロパティに移動します。
1. Launch UI の「**ルール**」セクションに移動し、「**ルールを追加**」をクリックします。
1. 「**CTA クリック済み**」という名前をルールに付けます。
1. **イベント**／**追加**&#x200B;をクリックして、**イベント設定**&#x200B;ウィザードを開きます。
1. **イベントタイプ**&#x200B;で、「**カスタムコード**」を選択します。

   ![「CTA クリック済み」という名前をルールに付け、カスタムコードイベントを追加](assets/track-clicked-component/custom-code-event.png)

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

   上記のコードスニペットで、データレイヤーに[関数をプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)して、イベントリスナーを追加します。`cmp:click` イベントがトリガーされると、`componentClickedHandler` 関数が呼び出されます。この関数では、いくつかの健全性チェックが追加され、新しい `event` オブジェクトが、イベントをトリガーしたコンポーネントの](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)データレイヤーの最新の状態[で構築されます。

   その後、`trigger(event)` が呼び出されます。`trigger()` は、Launch の予約名で、Launch ルールを「トリガー」します。Launch で `event` という別の予約名で公開される `event` オブジェクトをパラメーターとして渡します。Launch のデータ要素で、`event.component['someKey']` などの様々なプロパティを参照できるようになりました。

1. 変更を保存します。
1. 次に&#x200B;**アクション**&#x200B;で「**追加**」をクリックして&#x200B;**アクションの設定**&#x200B;ウィザードを開きます。
1. **アクションタイプ**&#x200B;で「**カスタムコード**」を選択します。

   ![カスタムコードアクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで「**編集画面を開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event` オブジェクトは、カスタムイベントで呼び出された `trigger()` メソッドから渡されます。`component` は、クリックをトリガーしたデータレイヤー `getState` から派生したコンポーネントの現在の状態です。

1. 変更を保存し、Launch で[ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html)を実行して、AEM サイトで使用する[環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja)にコードを昇格します。

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html?lang=ja) を使用して埋め込みコードを&#x200B;**開発**&#x200B;環境に切り替えると非常に便利です。

1. [WKND サイト](https://wknd.site/us/en.html)に移動し、デベロッパーツールを開いてコンソールを表示します。「**ログを保持**」を選択します。

1. 「**ティーザー**」または「**ボタン**」のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![CTA クリックするボタン](assets/track-clicked-component/cta-button-to-click.png)

1. **CTA クリック済み**&#x200B;ルールが呼び出されたことを Developer Console で確認します。

   ![CTA ボタンクリック済み](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネント ID とタイトルを取り込みます。前の演習で確認したとおり、`event.path` の出力は `component.button-b6562c963d` に似たものであり、`event.component['dc:title']` の値は「View Trips」と同様のものでした。

### コンポーネント ID

1. Experience Platform Launch に移動し、AEM サイトと統合された web プロパティに移動します。
1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. 「**名前**」には、**コンポーネント ID** を入力します。
1. 「**データ要素タイプ**」には、**カスタムコード**&#x200B;を選択します。

   ![コンポーネント ID データ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. 「**編集画面を開く**」をクリックし、カスタムコードエディターに以下を入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   変更を保存します。

   >[!NOTE]
   >
   > `event` オブジェクトは、Launch で&#x200B;**ルール**&#x200B;をトリガーしたイベントに基づいて利用可能になり、スコープされます。データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。したがって、前の演習で作成した **CTA クリック済み**&#x200B;ルールと同様に、このデータ要素はルール内では安全に使用できますが、**&#x200B;他のコンテキストでは安全に使用できません。

### コンポーネントのタイトル

1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. 「**名前**」には、**コンポーネントタイトル**&#x200B;と入力します。
1. 「**データ要素タイプ**」には、**カスタムコード**&#x200B;を選択します。
1. 「**編集画面を開く**」をクリックし、カスタムコードエディターに以下を入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   変更を保存します。

## CTA クリック済みルールに条件を追加する

次に、**CTA クリック済み**&#x200B;ルールを更新して、`cmp:click` イベントが&#x200B;**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;に対して発生した場合にのみルールが呼び出されるようにします。ティーザーの CTA はデータレイヤー内で別のオブジェクトと見なされるので、親を調べてティーザーから来たことを確認します。

1. Launch UI で、以前に作成した **CTA クリック済み**&#x200B;ルールに移動します。
1. **条件**&#x200B;で「**追加**」をクリックして&#x200B;**条件の設定**&#x200B;ウィザードを開きます。
1. **条件タイプ**&#x200B;で「**カスタムコード**」を選択します。

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

   上記のコードでは、最初にリソースタイプが&#x200B;**ボタン**&#x200B;から来たことを確認し、次にリソースタイプが&#x200B;**ティーザー**&#x200B;内の CTA から来たことを確認しています。

1. 変更を保存します。

## Analytics 変数の設定とリンクの追跡ビーコンのトリガー

現在、**CTA クリック済み**&#x200B;ルールは単にコンソールステートメントを出力します。次に、データ要素と Analytics 拡張機能を使用して、Analytics 変数を&#x200B;**アクション**&#x200B;として設定します。また、**リンクを追跡**&#x200B;をトリガーし、収集したデータを Adobe Analytics に送信する追加のアクションを設定します。

1. **CTA クリック済み**&#x200B;ルールで&#x200B;**コア - カスタムコード**&#x200B;アクション（コンソールステートメント）を&#x200B;**削除します**。

   ![カスタムコードアクションを削除](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」で、「**追加**」をクリックして新しいアクションを追加します。
1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**変数を設定**&#x200B;に設定します。

1. 「**eVar**」、「**Prop**」および「**イベント**」に次の値を設定します。

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar、Prop およびイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここで `%Component ID%` が使用されているのは、クリックされた CTA の一意の識別子を保証するからです。`%Component ID%` を使用することの潜在的な欠点は、Analytics レポートに `button-2e6d32893a` のような値が含まれることです。`%Component Title%` を使用すると、よりわかりやすい名前を付けることができますが、値が一意でない可能性があります。

1. 次に、**プラス**&#x200B;アイコンをタップして、**Adobe Analytics - 変数を設定**&#x200B;の右側にアクションを追加します。

   ![Launch アクションの追加](assets/track-clicked-component/add-additional-launch-action.png)

1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**ビーコンを送信**&#x200B;に設定します。
1. 「**トラッキング**」で、ラジオボタンを **`s.tl()`** に設定します。
1. 「**リンクタイプ**」に&#x200B;**カスタムリンク**&#x200B;を選択し、「**リンク名**」の値を **`%Component Title%: CTA Clicked`** に設定します。

   ![リンクビーコン送信の設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   これで、データ要素&#x200B;**コンポーネントタイトル**&#x200B;の動的変数と静的文字列 **CTA クリック済み**&#x200B;が結合されます。

1. 変更を保存します。**CTA クリック済み**&#x200B;ルールの設定は、次のようになります。

   ![最終的な Launch 設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** `cmp:click` イベントをリッスンします。
   * **2.** イベントが&#x200B;**ボタン**&#x200B;または&#x200B;**ティーザー**&#x200B;にトリガーされたことを確認します。
   * **3.** **コンポーネント ID** を **eVar**、**prop** および&#x200B;**イベント** として追跡する Analytics 変数を設定します。
   * **4.** Analytics のリンク追跡ビーコンを送信します（ページビューとして&#x200B;**扱わないようにします**）。

1. すべての変更を保存し、Experience Platform Launch ライブラリを構築して、適切な環境に昇格します。

## リンク追跡ビーコンと Analytics 呼び出しの検証

**CTA クリック済み**&#x200B;ルールで Analytics ビーコンを送信するようになったので、Experience Platform デバッガーを使用して Analytics トラッキング変数を確認できます。

1. ブラウザーで [WKND サイト](https://wknd.site/us/en.html)を開きます。
1. デバッガーアイコン ![Experience Platform デバッガーアイコン](assets/track-clicked-component/experience-cloud-debugger.png) をクリックして、Experience Platform デバッガーを開きます。
1. 前述のように、デバッガーで Launch プロパティが&#x200B;*自分の*&#x200B;開発環境にマッピングされており、「**コンソールログ**」がオンになっていることを確認します。
1. Analytics メニューを開き、レポートスイートが&#x200B;*自分の*&#x200B;レポートスイートに設定されていることを確認します。

   ![デバッガーの「Analytics」タブ](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![クリックする CTA ボタン](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform デバッガーに戻り、下にスクロールして&#x200B;**ネットワークリクエスト**／*レポートスイート*&#x200B;を展開します。**eVar**、**prop** および&#x200B;**イベント**&#x200B;が設定されていることがわかります。

   ![クリック時に追跡される Analytics のイベント、eVar および prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。サイトのフッターに移動し、ナビゲーションリンクのいずれかをクリックします。

   ![フッターのナビゲーションリンクのクリック](assets/track-clicked-component/click-navigation-link-footer.png)

1. *ルール「CTA クリック済み」の「カスタムコード」が満たされませんでした*&#x200B;というメッセージがブラウザーコンソールに表示されます。

   これは、ナビゲーションコンポーネントが `cmp:click` イベントをトリガーします&#x200B;*が*、リソースタイプの確認の結果、アクションが実行されないからです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、Experience Platform デバッガーの **Launch** で「**コンソールログ**」がオンになっていることを確認します。

## おめでとうございます。

イベント駆動型の Adobe Client Data Layer と Experience Platform Launch を使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡しました。
