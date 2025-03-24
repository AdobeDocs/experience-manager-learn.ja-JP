---
title: クリックされたコンポーネントの Adobe Analytics での追跡
description: イベント駆動型の Adobe Client Data Layer を使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックを追跡します。タグルールを使用してこれらのイベントをリッスンし、リンクのトラックビーコンを使用してAdobe Analytics レポートスイートにデータを送信する方法について説明します。
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="統合" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
duration: 394
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 100%

---

# クリックされたコンポーネントの Adobe Analytics での追跡

イベント駆動型の [Adobe Client Data Layer を AEM コアコンポーネントと共に](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)使用して、Adobe Experience Manager サイト上にある特定のコンポーネントのクリックをトラックします。タグプロパティのルールを使用してクリックイベントをリッスンし、コンポーネントでフィルタリングして、リンクのトラックビーコンと共にデータを Adobe Analytics に送信する方法について説明します。

## 作ろうとしているもの {#what-build}

WKND マーケティングチームは、どの `Call to Action (CTA)` ボタンがホームページで最も効果が高いのかを把握したいと考えています。このチュートリアルで、**ティーザー**&#x200B;および&#x200B;**ボタン**&#x200B;コンポーネントからの `cmp:click` イベントをリッスンするルールをタグプロパティに追加しましょう。次に、リンクのトラックビーコンとコンポーネント ID と新しいイベントを Adobe Analytics に送信します。

![クリックの追跡を作成する方法](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目的 {#objective}

1. `cmp:click` イベントをキャプチャするイベント駆動型のルールをタグプロパティに作成します。
1. コンポーネントリソースタイプで様々なイベントをフィルタリングします。
1. コンポーネント ID を設定し、リンクのトラックビーコンとイベントを Adobe Analytics に送信します。

## 前提条件

このチュートリアルは、[Adobe Analytics でページデータを収集](./collect-data-analytics.md)の続きであり、以下があることを前提としています。

* [Adobe Analytics 拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ja)で **タグプロパティ**&#x200B;が有効になっている
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバーがある。[レポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=ja)については、次のドキュメントを参照してください。
* タグプロパティが設定されている [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja) ブラウザー拡張機能が [WKND サイト](https://wknd.site/us/en.html)または Adobe Data Layer が有効になっている AEM サイトに読み込まれている。

## ボタンとティーザーのスキーマの検査

タグプロパティでルールを作成する前に、[ボタンとティーザーのスキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item)を確認して、データレイヤーの実装で検査します。

1. [WKND ホームページ](https://wknd.site/us/en.html)に移動します
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に移動します。次のコマンドを実行します。

   ```js
   adobeDataLayer.getState();
   ```

   上記のコードは、Adobe Client Data Layer の現在の状態を返します。

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

   上記のデータの詳細は、[コンポーネント／コンテナ項目スキーマ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#item)に基づいています。新しいタグルールはこのスキーマを使用します。

## CTA クリック済みルールの作成

Adobe Client Data Layer は、**イベント**&#x200B;駆動型のデータレイヤーです。任意のコアコンポーネントがクリックされると、データレイヤーを介して `cmp:click` イベントがディスパッチされます。`cmp:click` イベントをリッスンするために、ルールを作成しましょう。

1. Experience Platform に移動し、AEM サイトと統合されているタグプロパティに移動します。
1. タグプロパティ UI の「**ルール**」セクションに移動し、「**ルールを追加**」をクリックします。
1. 「**CTA クリック済み**」という名前をルールに付けます。
1. **イベント**／**追加**&#x200B;をクリックして、**イベント設定**&#x200B;ウィザードを開きます。
1. 「**イベントタイプ**」フィールドでは、「**カスタムコード**」を選択します。

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

   上記のコードスニペットで、データレイヤーに[関数をプッシュ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)して、イベントリスナーを追加します。`cmp:click` イベントがトリガーされるたびに、`componentClickedHandler` 関数が呼び出されます。この関数では、いくつかの健全性チェックが追加され、新しい `event` オブジェクトが、イベントをトリガーしたコンポーネントの[データレイヤーの最新の状態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)で構築されます。

   最後に、`trigger(event)` 関数が呼び出されます。この `trigger()` 関数は、タグプロパティ内の予約名で、ルールを&#x200B;**トリガー**&#x200B;します。`event` オブジェクトはパラメーターとして渡され、タグプロパティ内の別の予約名で公開されます。タグプロパティ内のデータ要素は、`event.component['someKey']` などのコードスニペットを使用して様々なプロパティを参照できるようになりました。

1. 変更を保存します。
1. 次に、**アクション**&#x200B;で「**追加**」をクリックして、**アクションの設定**&#x200B;ウィザードを開きます。
1. 「**アクションタイプ**」フィールドで、「**カスタムコード**」を選択します。

   ![カスタムコードアクションタイプ](assets/track-clicked-component/action-custom-code.png)

1. メインパネルで「**エディターを開く**」をクリックし、次のコードスニペットを入力します。

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event` オブジェクトは、カスタムイベントで呼び出された `trigger()` メソッドから渡されます。`component` オブジェクトは、データレイヤー `getState()` メソッドから派生したコンポーネントの現在の状態で、クリックをトリガーした要素です。

1. 変更を保存し、タグプロパティで[ビルド](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=ja)を実行して、AEM サイトで使用する[環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ja)にコードを昇格します。

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja) を使用して埋め込みコードを&#x200B;**開発**&#x200B;環境に切り替えると便利です。

1. [WKND サイト](https://wknd.site/us/en.html)に移動し、デベロッパーツールを開いてコンソールを表示します。また、「**ログを保持**」チェックボックスをオンにします。

1. 「**ティーザー**」または「**ボタン**」のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![CTA クリックするボタン](assets/track-clicked-component/cta-button-to-click.png)

1. **CTA クリック済み**&#x200B;ルールが呼び出されたことを Developer Console で確認します。

   ![CTA ボタンクリック済み](assets/track-clicked-component/cta-button-clicked-log.png)

## データ要素の作成

次に、データ要素を作成して、クリックされたコンポーネント ID とタイトルを取り込みます。前の演習で確認したとおり、`event.path` の出力は `component.button-b6562c963d` に似たものであり、`event.component['dc:title']` の値は「View Trips」と同様のものでした。

### コンポーネント ID

1. Experience Platform に移動し、AEM サイトと統合されているタグプロパティに移動します。
1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. 「**名前**」フィールドには、**コンポーネント ID** を入力します。
1. 「**データ要素タイプ**」フィールドは、「**カスタムコード**」を選択します。

   ![コンポーネント ID データ要素フォーム](assets/track-clicked-component/component-id-data-element.png)

1. 「**エディターを開く**」をクリックし、カスタムコードエディターに以下を入力します。

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 変更を保存します。

   >[!NOTE]
   >
   > `event` オブジェクトは、タグプロパティの&#x200B;**ルール**&#x200B;をトリガーしたイベントに基づいて利用可能になり、スコープが設定されることを思い出してください。データ要素の値は、データ要素がルール内で&#x200B;*参照*&#x200B;されるまで設定されません。したがって、このデータ要素は、前のステップで作成された&#x200B;**ページの読み込み**&#x200B;ルールなどのルール内で安全に使用できます。*ただし*、他のコンテキストでは安全に使用できません。


### コンポーネントのタイトル

1. 「**データ要素**」セクションに移動して、「**新規データ要素を追加**」をクリックします。
1. 「**名前**」フィールドで、**コンポーネントタイトル**&#x200B;と入力します。
1. 「**データ要素タイプ**」フィールドで、「**カスタムコード**」を選択します。
1. 「**エディターを開く**」ボタンをクリックし、カスタムコードエディターに以下を入力します。

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 変更を保存します。

## CTA クリック済みルールに条件を追加する

次に、**CTA クリック済み**&#x200B;ルールを更新して、`cmp:click` イベントが&#x200B;**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;に対して発生した場合にのみルールが呼び出されるようにします。ティーザーの CTA はデータレイヤー内で別のオブジェクトと見なされるので、親を調べてティーザーから来たことを確認します。

1. タグプロパティ UI で、以前に作成した **CTA クリック済み**&#x200B;ルールに移動します。
1. 「**条件**」で「**追加**」をクリックすると、**条件設定**&#x200B;ウィザードが表示されます。
1. 「**条件タイプ**」フィールドで、「**カスタムコード**」を選択します。

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

現在、**CTA クリック済み**&#x200B;ルールは単にコンソールステートメントを出力します。次に、データ要素と Analytics 拡張機能を使用して、Analytics 変数を&#x200B;**アクション**&#x200B;として設定します。また、**リンクをトラック**&#x200B;をトリガーし、収集したデータを Adobe Analytics に送信する追加のアクションを設定します。

1. **CTA クリック済み**&#x200B;ルールで&#x200B;**コア - カスタムコード**&#x200B;アクション（コンソールステートメント）を&#x200B;**削除します**。

   ![カスタムコードアクションを削除](assets/track-clicked-component/remove-console-statements.png)

1. 「アクション」で、「**追加**」をクリックしてアクションを作成します。
1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**変数を設定**&#x200B;に設定します。

1. 「**eVar**」、「**Prop**」および「**イベント**」に次の値を設定します。

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar、Prop およびイベントの設定](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > ここで `%Component ID%` が使用されているのは、クリックされた CTA の一意の ID を保証するからです。`%Component ID%` を使用することの潜在的な欠点は、Analytics レポートに `button-2e6d32893a` のような値が含まれることです。`%Component Title%` を使用すると、よりわかりやすい名前を付けることができますが、値が一意でない可能性があります。

1. 次に、**Adobe Analytics - 変数を設定**&#x200B;の右側に、**プラス**&#x200B;アイコンをタップしてさらにアクションを追加します。

   ![タグルールにさらにアクションを追加](assets/track-clicked-component/add-additional-launch-action.png)

1. 「**拡張機能**」タイプを **Adobe Analytics** に設定し、「**アクションタイプ**」を&#x200B;**ビーコンを送信**&#x200B;に設定します。
1. 「**トラッキング**」で、ラジオボタンを **`s.tl()`** に設定します。
1. 「**リンクタイプ**」フィールドで、「**カスタムリンク**」を選択し、「**リンク名**」の値を **`%Component Title%: CTA Clicked`** に設定します。

   ![リンクビーコンを送信の設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   上記の設定で、データ要素&#x200B;**コンポーネントタイトル**&#x200B;の動的変数と静的文字列 **CTA クリック済み**&#x200B;が結合されます。

1. 変更を保存します。**CTA クリック済み**&#x200B;ルールの設定は、次のようになります。

   ![最終的なタグルール設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.**`cmp:click` イベントをリッスンします。
   * **2.** イベントが&#x200B;**ボタン**&#x200B;または&#x200B;**ティーザー**&#x200B;にトリガーされたことを確認します。
   * **3.** **コンポーネント ID** を **eVar**、**prop** および&#x200B;**イベント**&#x200B;としてトラックする Analytics 変数を設定します。
   * **4.** Analytics のリンク追跡ビーコンを送信します（ページビューとして&#x200B;**扱わないようにします**）。

1. すべての変更を保存し、タグライブラリを構築して、適切な環境に昇格します。

## リンク追跡ビーコンと Analytics 呼び出しの検証

**CTA クリック済み**&#x200B;ルールで Analytics ビーコンを送信するようになったので、Experience Platform デバッガーを使用して Analytics トラッキング変数を確認できます。

1. ブラウザーで [WKND サイト](https://wknd.site/us/en.html)を開きます。
1. ![「Experience Platform Debugger」アイコン](assets/track-clicked-component/experience-cloud-debugger.png) の「デバッガー」アイコンをクリックして Experience Platform Debugger を起動します。
1. 前述のように、Debugger がタグプロパティを&#x200B;*ご自身の*&#x200B;開発環境にマッピングし、「**コンソールログ**」がオンになることを確認します。
1. Analytics メニューを開き、レポートスイートが&#x200B;*自分の*&#x200B;レポートスイートに設定されていることを確認します。

   ![デバッガーの「Analytics」タブ](assets/track-clicked-component/analytics-tab-debugger.png)

1. ブラウザーで、**ティーザー**&#x200B;または&#x200B;**ボタン**&#x200B;のいずれかの CTA ボタンをクリックして、別のページに移動します。

   ![クリックする CTA ボタン](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform デバッガーに戻り、下にスクロールして&#x200B;**ネットワークリクエスト**／*レポートスイート*&#x200B;を展開します。**eVar**、**prop** および&#x200B;**イベント**&#x200B;が設定されていることがわかります。

   ![クリック時に追跡される Analytics のイベント、eVar および prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. ブラウザーに戻り、デベロッパーコンソールを開きます。サイトのフッターに移動し、ナビゲーションリンクのいずれかをクリックします。

   ![フッターのナビゲーションリンクのクリック](assets/track-clicked-component/click-navigation-link-footer.png)

1. *ルール「CTA クリック済み」の「カスタムコード」が満たされませんでした*&#x200B;というメッセージがブラウザーコンソールに表示されます。

   上記のメッセージは、ナビゲーションコンポーネントが `cmp:click` イベントをトリガーします&#x200B;*が*、リソースタイプを確認する[ルールの条件](#add-a-condition-to-the-cta-clicked-rule)によりアクションが実行されないからです。

   >[!NOTE]
   >
   > コンソールログが表示されない場合は、Experience Platform Debugger の **Experience Platform タグ**&#x200B;で「**コンソールログ**」がオンになっていることを確認します。

## おめでとうございます。

イベント駆動型の Adobe Client Data Layer と Experience Platform のタグを使用して、AEM サイト上にある特定のコンポーネントのクリックをトラックしました。
