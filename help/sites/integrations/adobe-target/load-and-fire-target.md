---
title: ターゲット呼び出しの読み込みと実行
description: 起動ルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからターゲット呼び出しを実行する方法を学びます。 ページ情報を取得し、パラメーターとして渡すには、Adobeクライアントデータレイヤーを使用します。このレイヤーを使用すると、Webページ上での訪問者の体験に関するデータを収集して保存でき、このデータに簡単にアクセスできます。
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 5%

---


# ターゲット呼び出し{#load-fire-target}を読み込んで起動

起動ルールを使用して、ページリクエストにパラメーターを読み込み、渡し、サイトページからターゲット呼び出しを実行する方法を学びます。 Webページ情報を取得し、パラメーターとして渡すには、Adobeクライアントデータレイヤーを使用します。このレイヤーを使用すると、Webページ上での訪問者の体験に関するデータを収集し、保存して、このデータに簡単にアクセスできます。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## ページ型ルール

Adobe・クライアント・データ・レイヤーは、イベント主導のデータ・レイヤーです。 AEMページデータレイヤーが読み込まれると、イベント`cmp:show`がトリガーされます。 このビデオでは、カスタムイベントを使用して`Launch Library Loaded`ルールが呼び出されます。 以下に、カスタムイベントおよびデータ要素に関するビデオで使用されているコードスニペットを示します。

### 表示されたカスタムページイベント{#page-event}

![イベント設定とカスタムコードを示すページ](assets/load-and-fire-target-call.png)

Launchプロパティで、新しい&#x200B;**イベント**&#x200B;を&#x200B;**ルール**&#x200B;に追加します

+ __拡張機能：__ コア
+ __イベントタイプ:__ カスタムコード
+ __名前：__ ページ表示イベントハンドラー（または説明的な内容）

__エディターを開く__&#x200B;ボタンをタップし、次のコードスニペットに貼り付けます。 このコード&#x200B;__は、__&#x200B;イベント設定&#x200B;__とその後の__&#x200B;アクション&#x200B;__に__&#x200B;追加する必要があります。

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

カスタム関数は`pageShownEventHandler`を定義し、AEM Core Componentsが発行するイベントをリッスンし、コアコンポーネントの関連情報を派生させ、イベントオブジェクトにパッケージ化し、派生したイベント情報をペイロードに含めて起動イベントをトリガーします。

起動ルールは、起動の`trigger(...)`関数を使用してトリガされます。この関数は、規則のイベントのカスタムコードスニペット定義内で使用できる&#x200B;____&#x200B;のみです。

`trigger(...)`関数は、イベントオブジェクトをパラメーターとして受け取り、Launch Data Elementsで公開され、Launchの`event`という名前の別の予約名で公開されます。 Launchのデータ要素は、`event.component['someKey']`のような構文を使用して、`event`オブジェクトからこのイベントオブジェクトのデータを参照できるようになりました。

`trigger(...)`がイベントのカスタムコードイベントタイプのコンテキスト外（例えば、アクション内）で使用された場合、Launchプロパティと統合されたWebサイトでJavaScriptエラー`trigger is undefined`がスローされます。


### データ要素

![データ要素](assets/data-elements.png)

「Adobe起動データ要素」は、カスタムページ表示イベント](#page-event)でトリガーされたイベントオブジェクト[のデータを、コア拡張機能のカスタムコードデータ要素タイプを介してAdobe Targetで使用可能な変数にマップします。

#### ページIDデータ要素

```
if (event && event.id) {
    return event.id;
}
```

このコードは、コアコンポーネントの一意の生成IDを返します。

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

#### mboxDisable Cookieが設定されていない場合のエラーメッセージ

![ターゲットcookieドメインエラー](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決策

ターゲットのお客様は、テストや概念配達確認の目的で、ターゲットを持つクラウドベースのインスタンスを使用する場合があります。 これらのドメインは、パブリックサフィックスリストの一部で、その他多数のドメインが含まれます。
`targetGlobalSettings()`を使用して`cookieDomain`設定をカスタマイズしない限り、これらのドメインを使用している場合、最新のブラウザーではcookieが保存されません。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 次の手順

+ [エクスペリエンスフラグメントをAdobe Targetに書き出し](./export-experience-fragment-target.md)

## サポートリンク

+ [Adobeクライアントデータレイヤードキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Adobeクライアントデータレイヤーおよびコアコンポーネントドキュメントの使用](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platformデバッガの概要](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)