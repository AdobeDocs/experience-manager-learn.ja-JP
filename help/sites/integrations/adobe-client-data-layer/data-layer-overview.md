---
title: AEM コアコンポーネントでの Adobe Client Data Layer の使用
description: Adobeクライアントデータレイヤーでは、Web ページでの訪問者のエクスペリエンスに関するデータを収集して保存し、このデータに簡単にアクセスできるようにする標準的な方法が導入されています。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 57%

---

# AEM コアコンポーネントでの Adobe Client Data Layer の使用 {#overview}

Adobeクライアントデータレイヤーでは、Web ページでの訪問者のエクスペリエンスに関するデータを収集して保存し、このデータに簡単にアクセスできるようにする標準的な方法が導入されています。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEM サイトで Adobe Client Data Layer を有効にしますか？[手順はこちらを参照してください](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja#installation-activation)。

## データレイヤーの探索

ブラウザーの開発者ツールやライブの [WKND 参照サイト](https://wknd.site/us/en.html)を使用するだけで、Adobe Client Data Layer の組み込み機能を把握できます。

>[!NOTE]
>
> 次のスクリーンショットは、Chrome ブラウザーから取得したものです。

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html) に移動します。
1. 開発者ツールを開き、**コンソール**&#x200B;で次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

   AEMサイト上のデータレイヤーの現在の状態を確認するには、応答を調べます。 ページとそれぞれのコンポーネントに関する情報が表示されます。

   ![Adobe Data Layer のレスポンス](assets/data-layer-state-response.png)

1. コンソールで次のように入力して、データオブジェクトをデータレイヤーにプッシュします。

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. もう一度 `adobeDataLayer.getState()` コマンドを実行し、`training-data` のエントリを検索します。
1. 次にパスのパラメーターを追加して、コンポーネントの特定の状態だけを返すようにします。

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![1 つのコンポーネントのデータエントリのみを返す](assets/return-just-single-component.png)

## イベントとの連携

ベストプラクティスは、データレイヤーのイベントに基づいて任意のカスタムコードをトリガーすることです。次に、様々なイベントの登録とリスニングを探します。

1. コンソールに次のヘルパーメソッドを入力します。

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   上記のコードは、 `event` オブジェクトと `adobeDataLayer.getState` メソッドを使用して、イベントをトリガーしたオブジェクトの現在の状態を取得します。 次に、ヘルパーメソッドは、 `filter` 現在の `dataObject` が返されるフィルター条件を満たす。

   >[!CAUTION]
   >
   > この演習の間では、ブラウザーを更新&#x200B;**しない**&#x200B;ことが重要です。さもないと、コンソール JavaScript が失われます。

1. 次に、**カルーセル**&#x200B;の中に&#x200B;**ティーザー**&#x200B;コンポーネントが表示されたときに呼び出されるイベントハンドラーを入力します。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   この `teaserShownHandler` 関数が `getDataObjectHelper` 関数を参照し、 `wknd/components/teaser` を `@type` を使用して、他のコンポーネントによってトリガーされたイベントを除外します。

1. 次に、データレイヤーにイベントリスナーをプッシュして、`cmp:show` イベントをリッスンします。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   この `cmp:show` イベントは、 **カルーセル**&#x200B;または **タブ** コンポーネント。

1. ページで、カルーセルスライドを切り替えて、コンソールステートメントを確認します。

   ![カルーセルを切り替えてイベントリスナーを表示する](assets/teaser-console-slides.png)

1. リスニングを停止するには `cmp:show` イベント、データレイヤーからイベントリスナーを削除します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. ページに戻り、カルーセルスライドを切り替えます。これ以上のステートメントがログに記録されず、イベントがリッスンされていないことを確認します。

1. 次に、ページ表示イベントがトリガーされたときに呼び出されるイベントハンドラーを作成します。

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   イベントのフィルタリングにリソースタイプ `wknd/components/page` が使用されていることに注意します。

1. 次に、データレイヤーにイベントリスナーをプッシュして `cmp:show` イベントをリッスンし、`pageShownHandler` を呼び出します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. ページデータで実行されたコンソールステートメントがすぐに表示されます。

   ![ページ表示データ](assets/page-show-console-data.png)

   この `cmp:show` イベントを呼び出すことができます。 ページは既に読み込まれているのに、なぜイベントハンドラーが起動したのか、と思うかもしれません。

   Adobeクライアントデータレイヤーの独自の機能の 1 つは、イベントリスナーを登録できることです **前** または **後** データレイヤーが初期化されたので、競合状態を回避するのに役立ちます。

   データレイヤーは、順に発生したすべてのイベントのキュー配列を保持します。 デフォルトでは、データレイヤーは、 **過去** および **将来**. イベントを過去または将来からフィルタリングできます。 [詳しくは、ドキュメントを参照してください](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 次の手順

学習を続ける方法は 2 つあります。まず、 [ページデータを収集し、Adobe Analyticsに送信します。](../analytics/collect-data-analytics.md) Adobeクライアントデータレイヤーの使用方法を示すチュートリアルです。 2 つ目の選択肢は、次の方法を学ぶことです [AEMコンポーネントでのAdobeクライアントデータレイヤーのカスタマイズ](./data-layer-customize.md)


## その他のリソース {#additional-resources}

* [Adobe Client Data Layer ドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Client Data Layer とコアコンポーネントのドキュメントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)
