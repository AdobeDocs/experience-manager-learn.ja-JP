---
title: AEM コアコンポーネントでの Adobe Client Data Layer の使用
description: Adobe Client Data Layer は、web ページでの訪問者エクスペリエンスに関するデータを収集および保存し、このデータに容易にアクセスするための標準的な方法を導入しています。Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
duration: 777
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 100%

---

# AEM コアコンポーネントでの Adobe Client Data Layer の使用 {#overview}

Adobe Client Data Layer は、web ページでの訪問者エクスペリエンスに関するデータを収集および保存し、このデータに容易にアクセスするための標準的な方法を導入しています。Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。

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

   AEM サイト上のデータレイヤーの現在の状態を確認するには、応答を調べます。ページとそれぞれのコンポーネントに関する情報が表示されます。

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

   上記のコードは、`event` オブジェクトを検査し、`adobeDataLayer.getState` メソッドを使用して、イベントをトリガーしたオブジェクトの現在の状態を取得します。次に、ヘルパーメソッドは `filter` を検査し、現在の `dataObject` がフィルター条件を満たす場合にのみ返されます。

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

   `teaserShownHandler` 機能は `getDataObjectHelper` 機能を呼び出して、`wknd/components/teaser` のフィルターを `@type` として渡し、他のコンポーネントによって引き起こされたイベントを除外します。

1. 次に、データレイヤーにイベントリスナーをプッシュして、`cmp:show` イベントをリッスンします。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show` イベントは、**カルーセル**&#x200B;で新しいスライドが表示されたとき、**タブ**&#x200B;コンポーネントで新しいタブが選択されたときなど、多くの異なるコンポーネントでトリガーされます。

1. ページで、カルーセルスライドを切り替え、コンソールステートメントを確認します。

   ![カルーセルを切り替えてイベントリスナーを表示する](assets/teaser-console-slides.png)

1. `cmp:show` イベントのリッスンを停止するには、データレイヤーからイベントリスナーを削除します。

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

   ページの `cmp:show` イベントは、それぞれのページのロード時に、ページの一番上にトリガーされます。ページは既に読み込まれているのに、なぜイベントハンドラーが起動したのか、と思うかもしれません。

   Adobe Client Data Layer のユニークな機能の一つは、データレイヤーが初期化される&#x200B;**前**&#x200B;または&#x200B;**後**&#x200B;にイベントリスナーを登録できる点です。競合状態を回避するのに役立ちます。

   データレイヤーは、発生したすべてのイベントを順番に並べたキュー配列を保持します。データレイヤーは、デフォルトで、**過去**&#x200B;に発生したイベントと&#x200B;**未来**&#x200B;に発生するイベントに対して、イベントコールバックをトリガーします。イベントは、過去または未来でフィルタリングすることもできます。[詳しくは、ドキュメントを参照してください](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 次の手順

学習を続ける方法は 2 つあります。1 つ目の選択肢は、Adobe Client Data layer の使い方を説明したチュートリアル「[ページデータを収集して Adobe Analytics に送信する](../analytics/collect-data-analytics.md)」を学習することです。2 つ目の選択肢は、[AEM コンポーネントで Adobe Client Data Layer をカスタマイズする](./data-layer-customize.md)方法を学習することです。


## その他のリソース {#additional-resources}

* [Adobe Client Data Layer ドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Client Data Layer とコアコンポーネントのドキュメントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)
