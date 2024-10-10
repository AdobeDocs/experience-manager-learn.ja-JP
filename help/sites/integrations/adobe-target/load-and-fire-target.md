---
title: Target 呼び出しの読み込みと実行
description: タグルールを使用して、ページリクエストにパラメーターを読み込み、渡して、サイトページから Target 呼び出しを実行する方法について説明します。
feature: Core Components, Adobe Client Data Layer
version: Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 588
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '544'
ht-degree: 100%

---

# Target 呼び出しの読み込みと実行 {#load-fire-target}

タグルールを使用して、ページリクエストにパラメーターを読み込み、渡して、サイトページから Target 呼び出しを実行する方法について説明します。 web ページの情報を取得し、Adobe Client Data Layer を使用してパラメーターとして渡すことで、web ページでの訪問者の体験に関するデータを収集、保存し、そのデータに容易にアクセスできるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## ページ読み込みルール

Adobe Client Data Layer は、イベント駆動型のデータレイヤーです。 AEM ページデータレイヤーが読み込まれると、`cmp:show` イベントがトリガーされます。 ビデオでは、カスタムイベントを使用して `tags Library Loaded` ルールを呼び出しています。 次に、カスタムイベントとデータ要素に使用されるコードスニペットを示します。

### カスタムページ表示イベント{#page-event}

![表示されるページのイベント設定とカスタムコード](assets/load-and-fire-target-call.png)

タグプロパティで、新しい&#x200B;**イベント**&#x200B;を&#x200B;**ルール**&#x200B;に追加します。

+ __拡張機能：__&#x200B;コア
+ __イベントタイプ：__&#x200B;カスタムコード
+ __名前：__ Page Show Event Handler（または説明的なもの）

「__エディターを開く__」ボタンをタップして、次のコードスニペットをペーストします。 このコードは&#x200B;__必須__&#x200B;で、__イベント設定__&#x200B;とそれに続く&#x200B;__アクション__&#x200B;に追加する必要があります。

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

カスタム関数は `pageShownEventHandler` を定義し、AEM コアコンポーネントが発するイベントをリッスンし、コアコンポーネントの関連情報を導き出し、イベントオブジェクトにパッケージし、ペイロードに導き出したイベント情報でタグイベントをトリガーします。

タグルールはタグの `trigger(...)` 関数を使用してトリガーされます。この関数は、ルールのイベント のカスタムコードのコードスニペット定義内で&#x200B;__のみ__&#x200B;利用可能です。

`trigger(...)` 関数は、イベントオブジェクトをパラメーターとして受け取ります。このオブジェクトは、タグデータ要素の中で、`event` というタグの別の予約名で公開されています。 タグのデータ要素は、`event.component['someKey']` のような構文を使用して、`event` オブジェクトからこのイベントオブジェクトのデータを参照できるようになりました。

`trigger(...)` がイベントのカスタムコードイベントタイプのコンテキスト外（例えばアクション内）で使用された場合、JavaScript エラー `trigger is undefined` がタグプロパティと統合された web サイト上で投げられます。


### データ要素

![データ要素](assets/data-elements.png)

タグデータ要素は、[カスタムページ表示イベントでトリガーされた](#page-event)イベントオブジェクトのデータを、コア拡張機能のカスタムコードデータ要素タイプを通じて、Adobe Target で利用可能な変数にマッピングします。

#### ページ ID データ要素

```
if (event && event.id) {
    return event.id;
}
```

このコードは、コアコンポーネントの一意の ID を生成して返します。

![ページ ID](assets/pageid.png)

### ページパスデータ要素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

このコードは、AEM ページのパスを返します。

![ページパス](assets/pagepath.png)

### ページタイトルデータ要素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

このコードは、AEM ページのタイトルを返します。

![ページタイトル](assets/pagetitle.png)

## トラブルシューティング

### web ページで mbox が実行されないのはなぜですか。

#### mboxDisable Cookie が設定されていない場合のエラーメッセージ

![Target の Cookie ドメインエラー](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### ソリューション

Target のお客様は、Target でクラウドベースのインスタンスを使用してテストをおこなったり、シンプルな概念実証に利用したりする場合があります。 これらのドメインは、その他多くが、パブリックサフィックスリストに含まれています。
最近のブラウザーでは、これらのドメインを使用している場合、`targetGlobalSettings()` を使用して `cookieDomain` 設定をカスタマイズしない限り、Cookie は保存されません。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 次の手順

+ [Adobe Target へのエクスペリエンスフラグメントの書き出し](./export-experience-fragment-target.md)

## サポートリンク

+ [Adobe Client Data Layer ドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Client Data Layer とコアコンポーネントのドキュメントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)
+ [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja)
