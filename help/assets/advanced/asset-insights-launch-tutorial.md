---
title: AEM AssetsとAsset Launchを使用したアセットインサイトのAdobe
description: この5部構成のビデオシリーズでは、Launch by Adobeを通じてデプロイされたExperience Manager用のアセットインサイトの設定と設定について説明します。
feature: アセットインサイト
version: 6.3, 6.4, 6.5
topic: 統合
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 2%

---


# AEM AssetsとAdobe Experience Platform Launchを使用したアセットインサイトの設定

この5部構成のビデオシリーズでは、AdobeLaunchを介してデプロイされたExperience Manager用のアセットインサイトの設定手順を説明します。

## パート1:アセットインサイトの概要 {#overview}

アセットインサイトの概要 環境の準備を行うには、コアコンポーネント、サンプル画像コンポーネントおよびその他のコンテンツパッケージをインストールします。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### アーキテクチャ図 {#architecture-diagram}

![アーキテクチャ図](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>実装に合わせて、[最新バージョンのコアコンポーネント](https://github.com/adobe/aem-core-wcm-components)を必ずダウンロードしてください。

このビデオでは、最新バージョンではなくなったコアコンポーネントv2.2.2を使用しています。次の節に進む前に、必ず最新バージョンを使用してください。

* [アセットインサイトサンプル画像コンテンツ](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)のダウンロード
* [最新のAEM WCMコアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases)をダウンロードします。

## パート2 :サンプル画像コンポーネントのアセットインサイトトラッキングの有効化{#sample-image-component-asset-insights}

コアコンポーネントの機能強化と、アセットインサイト用のプロキシコンポーネント（サンプル画像コンポーネント）の使用。 コンテンツページテンプレートポリシーを編集して、リファレンスサイトのサンプル画像コンポーネントを有効にする。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>画像コアコンポーネントには、アセットのUUID（JCR内で作成されたノードの一意の識別子値）の追跡を無効にしてUUIDの追跡を無効にする機能が含まれています

コア画像コンポーネントは、この機能を有効または無効にするために、画像タグの親&lt;div>内で&#x200B;***data-asset-id***&#x200B;属性を使用します。 プロキシコンポーネントは、次の変更を加えてコアコンポーネントを上書きします。

* image.html内の&lt;img>要素の親divから&#x200B;***data-asset-id***&#x200B;を削除します。
* ***data-aem-asset-id***&#x200B;をimage.html内の&lt;img>要素に直接追加します。
* image.html内の&lt;img>要素に&#x200B;***data-trackable=&#39;true&#39;***&#x200B;値を追加します。
* ***data-aem-asset-*** idand  ***data-trackable=&#39;true&#39;*** は、同じノードレベルに保持されます。

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* と *data-trackable=&#39;true&#39;* は、アセットインプレッションに対して存在する必要がある主要な属性です。アセットクリックインサイトの場合、&lt;img>タグに存在する上記のデータ属性に加えて、親&lt;a>タグに有効なhref値が含まれている必要があります。

## パート3:Adobe Analytics — レポートスイートの作成。リアルタイムデータ収集とAEM Assetsレポートを有効にします。{#adobe-analytics-asset-insights}

リアルタイムデータ収集を含むレポートスイートが、アセット追跡用に作成されます。 AEM Assets Insights設定は、Adobe Analyticsの資格情報を使用して設定されます。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
Adobe Analyticsレポートスイートでリアルタイムデータ収集とAEM Asset Reportingを有効にする必要があります。 AEM Asset Reportingを有効にすると、アセットインサイトを追跡するための分析変数が予約されます。

AEM Assets Insights設定の場合は、次の資格情報が必要です

* データセンター
* Analytics会社名
* Analyticsユーザー名
* 共有暗号鍵(*Adobe Analytics/管理者/カンパニー設定/ Webサービス*&#x200B;から取得できます)。
* レポートスイート（アセットレポートに使用する適切なレポートスイートを必ず選択してください）

## パート4:Adobe Experience Platform Launchを使用したAdobe Analytics拡張機能の追加{#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adobe Analytics拡張機能の追加、ページ読み込みルールの作成、AEMとLaunchの統合(AdobeIMSテクニカルアカウントを使用)。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
すべての変更をオーサーインスタンスからパブリッシュインスタンスにレプリケートしてください。

### ルール1 :ページトラッカー(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

ページトラッカーが2つのコールバックを実装する（asset-embed-codeに登録）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>:asset-DOM-elementに対して「load」イベントがディスパッチされた場合に呼び出されます。&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;/code>** &lt;code>&lt;code>:asset-DOM-elementに対して「click」イベントがディスパッチされた場合に呼び出されます。これは、asset-DOM-elementに、有効な外部の「href」属性を持つアンカータグが親として含まれている場合にのみ関係します。&lt;/code>&lt;/code>

最後に、Pagetrackerは初期化関数をとして実装します。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:このメソッドは、Pagetrackerコンポーネントを初期化するために呼び出されます。&lt;/code>&lt;/code> この呼び出しは、Webページからasset-insights-events（インプレッション数やクリック数）が生成される前におこなう必要があります。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:オプションで、AppMeasurementオブジェクトを受け入れます。指定された場合、AppMeasurementオブジェクトの新しいインスタンスの作成は試行されません。&lt;/code>&lt;/code>

### ルール2:画像トラッカー — アクション1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### ルール2:画像トラッカー — アクション2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() :は、ページ読み込みの完了時に呼び出され、すべてのトラッキング可能な画像に対するトリガーアセットインプレッション数を示します。
* 読み込まれたアセットリストを運ぶAnalytics変数：**contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() :アセットのDOM要素に有効なhref値を持つアンカータグがある場合に呼び出されます。 アセットをクリックすると、クリックされたアセットIDを値として使用するCookieが作成されます。**(cookie名：a.assets.clickedid)**
* 読み込まれたアセットリストを運ぶAnalytics変数：**contextData[&#39;c.a.assets.clickedid&#39;]**
* 接触チャネル：**contextData[&#39;c.a.assets.source&#39;]**

### コンソールデバッグステートメント{#console-debug-statements}

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

Analyticsをデバッグする方法として、2つのGoogle Chromeブラウザー拡張機能がビデオで参照されます。 他のブラウザーでも同様の拡張機能を使用できます。

* [Launch Switch Chrome拡張機能](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

次のChrome拡張機能を使用して、DTMをデバッグモードに切り替えることもできます。[LaunchとDTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)。 これにより、DTMのデプロイメントに関連するエラーが発生したかどうかを簡単に確認できます。 さらに、次のスニペットを追加することで、任意のブラウザー&#x200B;*開発者ツール/JSコンソール*&#x200B;を使用して、DTMを手動でデバッグモードに切り替えることができます。

## パート5 :Analyticsの追跡とInsightデータの同期のテスト{#analytics-tracking-asset-insights}

AEM Assetレポートの同期ジョブスケジューラーとアセットインサイトレポートの設定

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
