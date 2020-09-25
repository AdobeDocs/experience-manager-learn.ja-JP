---
title: AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用
description: Adobeクライアントデータレイヤーでは、Webページ上での訪問者エクスペリエンスに関するデータを収集して保存し、そのデータに簡単にアクセスできるようにする標準的な方法が導入されています。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 6%

---


# Using the Adobe Client Data Layer with AEM Core Components {#overview}

Adobeクライアントデータレイヤーでは、Webページ上での訪問者エクスペリエンスに関するデータを収集して保存し、そのデータに簡単にアクセスできるようにする標準的な方法が導入されています。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEMサイトでAdobeクライアントデータレイヤーを有効にしますか。 [説明はこちら](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## データレイヤーの調査

ブラウザーおよびライブ [WKNDリファレンスサイトの開発者ツールを使用するだけで、Adobeクライアントデータレイヤーの組み込み機能を把握できます](https://wknd.site/)。

>[!NOTE]
>
> 以下に、Chromeブラウザーから取得したスクリーンショットを示します。

1. https://wknd.siteに移動し [ます。](https://wknd.site)
1. 開発者ツールを開き、 **コンソールに次のコマンドを入力します**。

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspectは、AEMサイトのデータ層の現在の状態を確認する応答です。 ページと個々のコンポーネントに関する情報が表示されます。

   ![Adobeデータレイヤーの応答](assets/data-layer-state-response.png)

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

1. コマンドを `adobeDataLayer.getState()` もう一度実行し、のエントリを探し `training-data`ます。
1. 次に、パスパラメーターを追加して、コンポーネントの特定の状態のみを返します。

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![単一のコンポーネントデータエントリのみを返す](assets/return-just-single-component.png)

## イベントの操作

データレイヤーからのイベントに基づいてカスタムコードをトリガーすることをお勧めします。 次に、様々なイベントの登録とリスニングを参照します。

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

   上記のコードは、 `event` オブジェクトを検査し、 `adobeDataLayer.getState` メソッドを使用して、イベントをトリガーしたオブジェクトの現在の状態を取得します。 次に、ヘルパーメソッドは、 `filter` 条件を検査し、現在の条件がフィルターに `dataObject` 一致する場合にのみ、その条件が返されます。

   >[!CAUTION]
   >
   > この実習全体を通してブラウザを更新し **ないことが重要です** 。更新しないと、コンソールJavaScriptが失われます。

1. 次に、カルーセル内にTeaser **コンポーネントが表示されたときに呼び出されるイベントハンドラを入力し******&#x200B;ます。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   は、メソッド `teaserShownHandler` を呼び出し、のフィルタを渡して、他のコンポーネントによってトリガーされたイベント `getDataObjectHelper``wknd/components/teaser``@type` を除外します。

1. 次に、イベントリスナーをデータレイヤーにプッシュして、 `cmp:show` イベントをリッスンします。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   この `cmp:show` イベントは、カルーセルに新しいスライドが表示された場合や、 **タブ****** コンポーネントで新しいタブが選択された場合など、様々なコンポーネントによってトリガされます。

1. ページでカルーセルスライドが切り替わり、コンソールの文を確認します。

   ![カルーセルを切り替えて、イベントリスナーを表示する](assets/teaser-console-slides.png)

1. イベントリスナーをデータレイヤーから削除して、 `cmp:show` イベントのリッスンを停止します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. ページに戻り、カルーセルスライドを切り替えます。 これ以上の文はログに記録されず、イベントが聞かれていないことを確認します。

1. 次に、ページ表示イベントがトリガされたときに呼び出されるイベントハンドラを入力します。

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   イベントのフィルタリングにリソースタイプ `wknd/components/page` が使用されることに注意してください。

1. 次に、イベントリスナーをデータレイヤーにプッシュして、 `cmp:show` イベントをリッスンし、を呼び出 `pageShownHandler`します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. ページデータでコンソール文が実行されたのがすぐにわかります。

   ![ページショーデータ](assets/page-show-console-data.png)

   ページの `cmp:show` イベントは、ページの最上部にある各ページの読み込み時にトリガーされます。 ページが明確に読み込まれているのに、イベントハンドラーがトリガーされたのはなぜか、と尋ねられることがあります。

   これは、データ層の初期化の **前または** 後にイベントリスナーを登録できるという点で、Adobeクライアントデータ層の独自の機能の1つです **** 。 これは、競合状態を回避するための重要な機能です。

   データレイヤーは、連続して発生したすべてのイベントのキュー配列を保持します。 データレイヤーは、 **過去に発生したイベントのイベントコールバックと** 将来のイベントをデフォルトでトリガします ****。 イベントをフィルターして過去または未来のみにすることができます。 [詳しくは、ドキュメントを参照してください](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 次の手順

イベント主導型のAdobeクライアントデータレイヤーを使用してページデータを [収集し、Adobe Analyticsに送信する方法については、次のチュートリアルを参照してください](../analytics/collect-data-analytics.md)。


## その他のリソース {#additional-resources}

* [Adobeクライアントデータレイヤードキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobeクライアントデータレイヤーおよびコアコンポーネントドキュメントの使用](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
