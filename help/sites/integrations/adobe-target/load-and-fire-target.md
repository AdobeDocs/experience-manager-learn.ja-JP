---
title: Target呼び出しの読み込みと実行
description: Launchルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからTarget呼び出しを実行する方法について説明します。 ページ情報は、Webページでの訪問者のエクスペリエンスに関するデータを収集して保存し、このデータに簡単にアクセスできるようにする、Adobeクライアントデータレイヤーを使用して取得および渡されます。
feature: コアコンポーネント、Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: 統合
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 5%

---


# Target呼び出し{#load-fire-target}の読み込みと実行

Launchルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからTarget呼び出しを実行する方法について説明します。 Webページの情報は、Webページでの訪問者のエクスペリエンスに関するデータを収集して保存し、このデータに容易にアクセスできるようにする、Adobeクライアントデータレイヤーを使用して取得および渡されます。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## ページ型ルール

Adobeクライアントデータレイヤーは、イベント駆動型データレイヤーです。 AEMページデータレイヤーが読み込まれると、イベント`cmp:show`がトリガーされます。 ビデオでは、`Launch Library Loaded`ルールがカスタムイベントを使用して呼び出されます。 以下に、カスタムイベントおよびデータ要素に関してビデオで使用されるコードスニペットを示します。

### カスタムページ表示イベント{#page-event}

![ページ表示イベント設定とカスタムコード](assets/load-and-fire-target-call.png)

Launchプロパティで、新しい&#x200B;**イベント**&#x200B;を&#x200B;**ルール**&#x200B;に追加します。

+ __拡張機能：__ Core
+ __イベントタイプ：__ カスタムコード
+ __名前：__ Page Show Event Handler（または説明的なもの）

「__エディターを開く__」ボタンをタップし、次のコードスニペットに貼り付けます。 このコード&#x200B;__は、__&#x200B;イベント設定&#x200B;__と、その後の__&#x200B;アクション&#x200B;__に追加する必要があります。__

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

カスタム関数は`pageShownEventHandler`を定義し、AEMコアコンポーネントから発行されるイベントをリッスンし、コアコンポーネントから関連情報を導出し、イベントオブジェクトにパッケージ化し、ペイロードに派生イベント情報を使用してLaunchイベントをトリガーします。

Launchルールは、Launchの`trigger(...)`関数を使用してトリガーされます。この関数は、ルールのイベントのカスタムコードスニペット定義内から使用できる&#x200B;__のみ__&#x200B;です。

`trigger(...)`関数は、イベントオブジェクトをパラメーターとして取り、Launchデータ要素で公開します。このパラメーターは、Launchの`event`という別の予約名です。 Launchのデータ要素は、`event.component['someKey']`のような構文を使用して、`event`オブジェクトからこのイベントオブジェクトのデータを参照できるようになりました。

`trigger(...)`がイベントのカスタムコードイベントタイプのコンテキスト外で（例えば、アクションで）使用された場合、Launchプロパティと統合されたWebサイトでJavaScriptエラー`trigger is undefined`が発生します。


### データ要素

![データ要素](assets/data-elements.png)

AdobeLaunchデータ要素は、カスタムページ表示イベント](#page-event)でトリガーされたイベントオブジェクト[のデータを、コア拡張機能のカスタムコードデータ要素タイプを介してAdobe Targetで使用可能な変数にマッピングします。

#### ページIDデータ要素

```
if (event && event.id) {
    return event.id;
}
```

このコードは、コアコンポーネントの一意のIDを生成して返します。

![ページID](assets/pageid.png)

### ページパスデータ要素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

このコードは、AEMページのパスを返します。

![ページパス](assets/pagepath.png)

### ページタイトルデータ要素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

このコードは、AEMページのタイトルを返します。

![ページタイトル](assets/pagetitle.png)

## トラブルシューティング

### Webページでmboxが実行されないのはなぜですか。

#### mboxDisable cookieが設定されていない場合のエラーメッセージ

![Targetのcookieドメインエラー](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決策

Targetのお客様は、テストや簡単な概念実証にTargetでクラウドベースのインスタンスを使用する場合があります。 これらのドメインやその他多くのドメインは、パブリックサフィックスリストに含まれています。
これらのドメインを使用する場合、`targetGlobalSettings()`を使用して`cookieDomain`設定をカスタマイズしない限り、最新のブラウザーではCookieが保存されません。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 次の手順

+ [エクスペリエンスフラグメントをAdobe Targetに書き出す](./export-experience-fragment-target.md)

## サポートリンク

+ [Adobeクライアントデータレイヤーのドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Adobeクライアントデータレイヤーとコアコンポーネントのドキュメントの使用](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debuggerの概要](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)