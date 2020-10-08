---
title: ターゲット呼び出しの読み込みと実行
description: 起動ルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからターゲット呼び出しを実行する方法を学びます。 ページ情報を取得し、パラメーターとして渡すには、Adobeクライアントデータレイヤーを使用します。このレイヤーを使用すると、Webページ上での訪問者の体験に関するデータを収集して保存でき、このデータに簡単にアクセスできます。
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 3%

---


# ターゲット呼び出しの読み込みと実行 {#load-fire-target}

起動ルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからターゲット呼び出しを実行する方法を学びます。 ページ情報を取得し、パラメーターとして渡すには、Adobeクライアントデータレイヤーを使用します。このレイヤーを使用すると、Webページ上での訪問者の体験に関するデータを収集して保存でき、このデータに簡単にアクセスできます。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## ページ型ルール

Adobe・クライアント・データ・レイヤーは、イベント主導のデータ・レイヤーです。 AEMページのデータレイヤーが読み込まれると、イベントがトリガーされ `cmp:show` ます。 このビデオでは、カスタムイベントを使用して `Launch Library Loaded` ルールが呼び出されます。 以下に、カスタムイベントおよびデータ要素に関するビデオで使用されているコードスニペットを示します。

### カスタムイベント

以下のコードスニペットでは、関数をデータレイヤーにプッシュしてイベントリスナーを追加します。 イベントがトリガされると、 `cmp:show` 関数が呼び出され `pageShownEventHandler` ます。 この関数では、いくつかのサニティチェックが追加され、イベントをトリガーしたコンポーネントのデータレイヤーの最新の状態 `dataObject` で新しく構築されます。

その後 `trigger(dataObject)` に。 `trigger()` は、起動で予約された名前で、起動ルールを「トリガー」します。 イベントオブジェクトをパラメータとして渡し、その後、Launchの名前付きイベントで別の予約名で公開されます。 起動のデータ要素は、次のような様々なプロパティを参照できるようになりました。 `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
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
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### データレイヤーページID

```
if(event && event.id) {
    return event.id;
}
```

![ページID](assets/pageid.png)

### ページパス

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![ページパス](assets/pagepath.png)

### ページタイトル

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![ページタイトル](assets/pagetitle.png)

### 一般的な問題

#### Webページでmboxが実行されないのはなぜですか。

**mboxDisable Cookieが設定されていない場合のエラーメッセージ**

![ターゲットcookieドメインエラー](assets/target-cookie-error.png)

**解決策**

ターゲットのお客様は、テストや概念配達確認の目的で、ターゲットを持つクラウドベースのインスタンスを使用する場合があります。 これらのドメインは、パブリックサフィックスリストの一部で、その他多数のドメインが含まれます。
これらのドメインを使用している場合、を使用して設定をカスタマイズしない限り、最新のブラウザーではcookieが保存されません `cookieDomain``targetGlobalSettings()`。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## 次の手順

1. [エクスペリエンスフラグメントをAdobe Targetに書き出し](./export-experience-fragment-target.md)

## サポートリンク

* [Adobeクライアントデータレイヤードキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Adobeクライアントデータレイヤーおよびコアコンポーネントドキュメントの使用](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Adobe Experience Platformデバッガの概要](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)