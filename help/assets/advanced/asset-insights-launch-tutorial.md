---
title: AEM Assetsと Asset Launch を使用した Asset Insights のAdobe
description: この 5 部構成のビデオシリーズでは、Launch by Adobeを通じてデプロイされたExperience Manager用の Asset Insights の設定と設定について説明します。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# AEM AssetsとAdobe Experience Platform Launchでの Asset Insights の設定

この 5 部構成のビデオシリーズでは、Asset Launch を通じてデプロイされたExperience Manager用の Asset Insights の設定と設定について説明します。

## パート 1:アセットインサイトの概要 {#overview}

アセットインサイトの概要。 環境の準備を整えるには、コアコンポーネント、サンプル画像コンポーネントおよびその他のコンテンツパッケージをインストールします。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### アーキテクチャ図 {#architecture-diagram}

![アーキテクチャ図](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>必ず [コアコンポーネントの最新バージョン](https://github.com/adobe/aem-core-wcm-components) を参照してください。

このビデオでは、最新バージョンではなくなったコアコンポーネント v2.2.2 を使用しています。次の節に進む前に、必ず最新バージョンを使用してください。

* ダウンロード [アセットインサイトサンプル画像コンテンツ](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* ダウンロード [最新のAEM WCM コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases)

## パート 2 :サンプル画像コンポーネントのアセットインサイトトラッキングの有効化 {#sample-image-component-asset-insights}

コアコンポーネントの機能強化と、アセットインサイトでのプロキシコンポーネント（サンプル画像コンポーネント）の使用。 コンテンツページテンプレートポリシーを編集して、リファレンスサイトのサンプル画像コンポーネントを有効にします。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>画像コアコンポーネントには、アセットの UUID（JCR 内で作成されたノードの一意の識別子値）の追跡を無効にして、UUID の追跡を無効にする機能が含まれています

コア画像コンポーネントはを使用します ***data-asset-id*** 親内の属性 &lt;div> をクリックして、この機能を有効/無効にします。 プロキシコンポーネントは、次の変更を加えて、コアコンポーネントを上書きします。

* を削除します。 ***data-asset-id*** image.html 内の要素 &lt;img> の親 div から
* 追加 ***data-aem-asset-id*** image.html 内の要 &lt;img> 素に直接アクセスできます。
* 追加 ***data-trackable=&#39;true&#39;*** 値を image.html 内 &lt;img> の要素に設定します。
* ***data-aem-asset-id*** および ***data-trackable=&#39;true&#39;*** 同じノードレベルに保持される

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* および *data-trackable=&#39;true&#39;* は、アセットインプレッションに存在する必要がある主要な属性です。 アセットクリックインサイトの場合、タグに存在する上記のデータ属性に加えて、親タグに &lt;img> 有効な href 値が含まれている必要があります。

## パート 3:Adobe Analytics — レポートスイートの作成。リアルタイムデータ収集とAEM Assetsレポートを有効にします。 {#adobe-analytics-asset-insights}

リアルタイムのデータ収集を含むレポートスイートが、アセット追跡用に作成されます。 AEM Assets Insights 設定は、Adobe Analytics資格情報を使用して設定されます。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
Adobe Analyticsレポートスイートでリアルタイムのデータ収集とAEM Asset Reporting を有効にする必要があります。 AEM Asset Reporting を有効にすると、アセットのインサイトを追跡するための Analytics 変数が予約されます。

AEM Assets Insights 設定の場合は、次の資格情報が必要です

* データセンター
* Analytics 会社名
* Analytics ユーザー名
* 共有暗号鍵 ( *Adobe Analytics /管理者/カンパニー設定/ Web サービス*) をクリックします。
* レポートスイート（アセットレポートに使用する適切なレポートスイートを必ず選択してください）

## パート 4:Adobe Experience Platform Launchを使用したAdobe Analytics拡張機能の追加 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adobe Analytics拡張機能の追加、ページ読み込みルールの作成、AEMと Launch の統合（統合テクニカルアカウント）。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
オーサーインスタンスからパブリッシュインスタンスに、すべての変更をレプリケートしてください。

### ルール 1 :ページトラッカー (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

ページトラッカーが 2 つのコールバックを実装している（asset-embed-code に登録されている）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** :asset-DOM-element に対して「load」イベントがディスパッチされた場合に呼び出されます。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** :asset-DOM-element に対して「click」イベントがディスパッチされたときに呼び出されます。これは、asset-DOM-element が、有効な外部の「href」属性を持つアンカータグを親として持つ場合にのみ関連します

最後に、ページトラッカーは初期化関数をとして実装します。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :このメソッドを呼び出して、Pagetracker コンポーネントを初期化します。 この呼び出しは、asset-insights-events（インプレッション数やクリック数）が Web ページから生成される前におこなう必要があります。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :オプションで AppMeasurement オブジェクトを受け入れます。指定された場合、AppMeasurement オブジェクトの新しいインスタンスの作成は試みません。

### ルール 2:画像トラッカー — アクション 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### ルール 2:画像トラッカー — アクション 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() :がページ読み込み完了時に呼び出され、はすべての追跡可能画像のトリガーアセットインプレッション数
* 読み込まれたアセットリストを運ぶ Analytics 変数： **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() :アセットの DOM 要素に有効な href 値を持つアンカータグがある場合に呼び出されます。 アセットをクリックすると、クリックされたアセット ID を値として持つ Cookie が作成されます。**(cookie 名：a.assets.clickedid)**
* 読み込まれたアセットリストを運ぶ Analytics 変数： **contextData[&#39;c.a.assets.clickedid&#39;]**
* 起源： **contextData[&#39;c.a.assets.source&#39;]**

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

Analytics のデバッグ方法として、2 つのGoogle Chrome ブラウザー拡張機能がビデオで参照されます。 他のブラウザーでも同様の拡張機能を使用できます。

* [Launch Switch Chrome 拡張機能](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

次の Chrome 拡張機能を使用して、DTM をデバッグモードに切り替えることもできます。 [Launch と DTM の切り替え](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). これにより、DTM のデプロイメントに関連するエラーが発生したかどうかを簡単に確認できます。 さらに、任意のブラウザーを使用して、DTM を手動でデバッグモードに切り替えることができます *開発者ツール/JS コンソール* 次のスニペットを追加する。

## パート 5 :Analytics の追跡と Insight データの同期のテスト{#analytics-tracking-asset-insights}

AEM Asset Reporting 同期ジョブスケジューラーとアセットインサイトレポートの設定

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
