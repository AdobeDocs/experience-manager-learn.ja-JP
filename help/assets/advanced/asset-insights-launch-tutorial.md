---
title: AEM Assets と Adobe Launch を使用したアセットインサイトのセットアップ
description: この 5 部構成のビデオシリーズでは、Launch By Adobe を介してデプロイされた Experience Manager 用のアセットインサイトのセットアップと設定について順を追って説明します。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Assets as a Cloud Service、AEM Assets 6.5" before-title="false"
doc-type: Tutorial
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
duration: 2051
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '811'
ht-degree: 100%

---

# AEM Assets と Adobe Experience Platform Launch を使用したアセットインサイトのセットアップ

この 5 部構成のビデオシリーズでは、Adobe Launch を介してデプロイされた Experience Manager 用のアセットインサイトのセットアップと設定について順を追って説明します。

## 第 1 部：アセットインサイトの概要 {#overview}

アセットインサイトの概要。 コアコンポーネント、サンプル画像コンポーネントおよびその他のコンテンツパッケージをインストールして、環境を準備します。

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### アーキテクチャ図 {#architecture-diagram}

![アーキテクチャ図](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>実装のために、必ず[コアコンポーネントの最新バージョン](https://github.com/adobe/aem-core-wcm-components)をダウンロードしてください。

このビデオでは、最新バージョンではないコアコンポーネント v2.2.2 を使用しています。次の節に進む前に、必ず最新バージョンを使用してください。

* [アセットインサイトのサンプル画像コンテンツ](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)のダウンロード
* [最新の AEM WCM コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases)のダウンロード

## 第 2 部：サンプル画像コンポーネントに対するアセットインサイトトラッキングの有効化 {#sample-image-component-asset-insights}

コアコンポーネントの機能強化と、アセットインサイトのプロキシコンポーネント（サンプル画像コンポーネント）の使用。コンテンツページテンプレートポリシーを編集して、リファレンスサイトに対してサンプル画像コンポーネントを有効にします。

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>画像コアコンポーネントには、アセットの UUID（JCR 内で作成されたノードの一意の ID の値）の追跡を無効にして UUID のトラッキングを無効にする機能が含まれています。

コア画像コンポーネントは、画像タグの親 &lt;div> 内で ***data-asset-id*** 属性を使用して、この機能を有効または無効にします。プロキシコンポーネントは、以下の変更でコアコンポーネントを上書きします。

* image.html 内の &lt;img> 要素の親 div から ***data-asset-id*** を削除します。
* ***data-aem-asset-id*** を image.html 内の &lt;img> 要素に直接追加します。
* ***data-trackable=&#39;true&#39;*** 値を image.html 内の &lt;img> 要素に追加します。
* ***data-aem-asset-id*** および ***data-trackable=&#39;true&#39;*** は、同じノードレベルに保たれます。

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* と *data-trackable=&#39;true&#39;* は、アセットインプレッションのために存在する必要がある主要な属性です。アセットクリックインサイトの場合、&lt;img> タグに存在する上記のデータ属性に加えて、有効な href 値が親の &lt;a> タグに必要です。

## 第 3 部：Adobe Analytics - レポートスイートの作成、リアルタイムデータ収集と AEM Assets レポートの有効化。 {#adobe-analytics-asset-insights}

リアルタイムデータ収集を伴うレポートスイートが、アセットトラッキング用に作成されます。 AEM アセットインサイトの設定は、Adobe Analytics 資格情報を使用してセットアップされます。

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
>Adobe Analytics レポートスイート用に、リアルタイムデータ収集と AEM アセットレポートを有効にする必要があります。AEM アセットレポートを有効にすると、アセットインサイトを追跡するための Analytics 変数が予約されます。

AEM アセットインサイトの設定の場合は、以下の資格情報が必要です。

* データセンター
* Analytics 会社名
* Analytics ユーザー名
* 共有暗号鍵（*Adobe Analytics／管理者／会社設定／web サービス*&#x200B;で取得可能）。
* レポートスイート（必ずアセットレポートへの使用に適したレポートスイートを選択してください）

## 第 4 部：Adobe Experience Platform Launch を使用した Adobe Analytics 拡張機能の追加 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adobe Analytics 拡張機能の追加、ページ読み込みルールの作成および Adobe IMS テクニカルアカウントを使用した AEMと Launch の統合。

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
>必ず、すべての変更をオーサーインスタンスからパブリッシュインスタンスにレプリケートしてください。

### ルール 1：ページトラッカー（pagetracker.js） {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

ページトラッカーは（asset-embed-code に登録されている）2 つのコールバックを実装しています。

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>**：asset-DOM-element に対して「load」イベントがディスパッチされたときに呼び出されます。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>**：asset-DOM-element に対して「click」イベントがディスパッチされたときに呼び出されます。これは、asset-DOM-element が、有効な外部の「href」属性を持つアンカータグを親として持つ場合にのみ該当します。

最後に、ページトラッカーは以下のような初期化関数を実装します。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>**：ページトラッカーコンポーネントを初期化するために呼び出されます。これは、アセットインサイトイベント（インプレッション数やクリック数）が web ページから生成される前に呼び出す必要があります。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>**：オプションで AppMeasurement オブジェクトを受け取ります。指定された場合は、AppMeasurement オブジェクトのインスタンスの作成を試みません。

### ルール 2：画像トラッカー - アクション 1（asset-insights.js） {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### ルール 2：画像トラッカー - アクション 2（image-tracker.js） {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded()：ページの読み込みが完了したときに呼び出され、追跡可能なすべての画像のアセットインプレッションをトリガーします。
* 読み込まれたアセットリストを保持する Analytics 変数：**contextData[「c.a.assets.idList」]**
* assetAnalytics.core.assetClicked()：アセットの DOM 要素に有効な href 値を持つアンカータグがある場合に呼び出されます。アセットをクリックすると、クリックされたアセット ID を値として持つ Cookie が作成されます。**（Cookie 名：a.assets.clickedid）**
* 読み込まれたアセットリストを保持する Analytics 変数：**contextData[「c.a.assets.clickedid」]**
* オリジンのソース：**contextData[「c.a.assets.source」]**

### コンソールデバッグステートメント {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

ビデオでは、Analytics をデバッグする方法として、2 つの Google Chrome ブラウザー拡張機能に言及しています。その他のブラウザーでも同様の拡張機能を使用できます。

* [Launch 切り替え Chrome 拡張機能](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=ja)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

Chrome 拡張機能「[Launch と DTM の切り替え](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=ja)」を使用して、DTM をデバッグモードに切り替えることもできます。これにより、DTM のデプロイメントに関連するエラーが発生したかどうかを簡単に確認できます。さらに、次のスニペットを追加することで、任意のブラウザーから&#x200B;*開発者ツール／JS コンソール*&#x200B;を通じて DTM を手動でデバッグモードに切り替えることができます。

## パート 5：Analytics トラッキングのテストとインサイトデータの同期{#analytics-tracking-asset-insights}

AEM Asset レポート同期ジョブスケジューラーとアセットインサイトレポートの設定

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
