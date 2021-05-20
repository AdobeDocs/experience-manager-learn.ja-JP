---
title: AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用
description: Adobeクライアントデータレイヤーは、Webページでの訪問者エクスペリエンスに関するデータを収集および保存し、このデータに簡単にアクセスできるようにする標準的な方法を導入します。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
topic: 統合
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 8%

---


# AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用 {#overview}

Adobeクライアントデータレイヤーは、Webページでの訪問者エクスペリエンスに関するデータを収集および保存し、このデータに簡単にアクセスできるようにする標準的な方法を導入します。 Adobe Client Data Layer はプラットフォームに依存しませんが、AEM で使用するためにコアコンポーネントに完全に統合されています。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEMサイトでAdobeクライアントデータレイヤーを有効にしますか？ [こちらの手順を参照してください](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## データレイヤーの調査

Adobeクライアントデータレイヤーの組み込み機能は、ブラウザーの開発者ツールとライブの[WKNDリファレンスサイト](https://wknd.site/)を使用するだけでわかります。

>[!NOTE]
>
> 以下は、Chromeブラウザーから撮影したスクリーンショットです。

1. [https://wknd.site](https://wknd.site)に移動します。
1. 開発者ツールを開き、**コンソール**&#x200B;に次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspectへの応答で、AEMサイト上のデータレイヤーの現在の状態を確認します。 ページと個々のコンポーネントに関する情報が表示されます。

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

1. コマンド`adobeDataLayer.getState()`をもう一度実行し、`training-data`のエントリを見つけます。
1. 次に、コンポーネントの特定の状態のみを返すパスパラメーターを追加します。

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![単一のコンポーネントのデータエントリのみを返す](assets/return-just-single-component.png)

## イベントの操作

データレイヤーのイベントに基づいてカスタムコードをトリガーすることをお勧めします。 次に、様々なイベントの登録とリスンを参照します。

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

   上記のコードは、`event`オブジェクトを調べ、`adobeDataLayer.getState`メソッドを使用して、イベントをトリガーしたオブジェクトの現在の状態を取得します。 次に、ヘルパーメソッドは、現在の`dataObject`がフィルターに一致する場合にのみ、`filter`条件を検査します。

   >[!CAUTION]
   >
   > この演習全体でブラウザーを更新する際には、****&#x200B;ではなくが重要です。そうしないと、コンソールのJavaScriptが失われます。

1. 次に、**ティーザー**&#x200B;コンポーネントが&#x200B;**カルーセル**&#x200B;内に表示されたときに呼び出されるイベントハンドラーを入力します。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   `teaserShownHandler`は`getDataObjectHelper`メソッドを呼び出し、`wknd/components/teaser`のフィルターを`@type`として渡して、他のコンポーネントによってトリガーされたイベントを除外します。

1. 次に、イベントリスナーをデータレイヤーにプッシュして`cmp:show`イベントをリッスンします。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show`イベントは、**カルーセル**&#x200B;に新しいスライドが表示される場合や、**タブ**&#x200B;コンポーネントで新しいタブが選択された場合など、様々なコンポーネントによってトリガーされます。

1. ページでカルーセルスライドを切り替え、コンソールステートメントを確認します。

   ![カルーセルを切り替えてイベントリスナーを表示](assets/teaser-console-slides.png)

1. `cmp:show`イベントのリッスンを停止するには、データレイヤーからイベントリスナーを削除します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. ページに戻り、カルーセルスライドを切り替えます。 これ以上ログに記録されず、イベントがリッスンされていないことを確認します。

1. 次に、ページ表示イベントがトリガーされたときに呼び出されるイベントハンドラーを入力します。

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   リソースタイプ`wknd/components/page`を使用してイベントをフィルターします。

1. 次に、イベントリスナーをデータレイヤーにプッシュして`cmp:show`イベントをリッスンし、`pageShownHandler`を呼び出します。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. ページデータでコンソールステートメントが実行されたのがすぐにわかります。

   ![ページショーデータ](assets/page-show-console-data.png)

   ページの`cmp:show`イベントは、各ページの読み込み時にページの最上部でトリガーされます。 ページが明確に読み込まれているのに、イベントハンドラーがトリガーされたのはなぜですか？と問う場合があります。

   これは、Adobeクライアントデータレイヤーの独自の機能の1つで、**または**&#x200B;の前に&#x200B;**イベントリスナーを登録できる点で、データレイヤーが初期化された後に**&#x200B;を登録できます。 これは、競合状態を回避するための重要な機能です。

   データレイヤーは、順に発生したすべてのイベントのキュー配列を保持します。 デフォルトでは、データレイヤーは、**過去**&#x200B;に発生したイベントと&#x200B;**将来**&#x200B;に発生したイベントのイベントコールバックをトリガーします。 イベントを過去または将来のみにフィルタリングできます。 [詳しくは、](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)ドキュメントを参照してください。


## 次の手順

イベントドリブン型Adobeクライアントデータレイヤーを使用して[ページデータを収集し、Adobe Analytics](../analytics/collect-data-analytics.md)に送信する方法については、次のチュートリアルを参照してください。

または、[AEMコンポーネントでAdobeクライアントデータレイヤーをカスタマイズする方法を説明します。](./data-layer-customize.md)


## その他のリソース {#additional-resources}

* [Adobeクライアントデータレイヤーのドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobeクライアントデータレイヤーとコアコンポーネントのドキュメントの使用](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
