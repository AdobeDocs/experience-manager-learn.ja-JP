---
title: 統合Experience PlatformWeb SDK
description: AEM as a Cloud ServiceをExperience PlatformWeb SDK と統合する方法について説明します。 この基本的なステップは、Adobe Analytics、Target などのAdobe Experience Cloud製品や、Real-time Customer Data Platform、Customer Journey Analytics、Journey Optimizerなどの最近の革新的な製品を統合するために不可欠です。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 63afa03de70d6f8f695d552018344d53a5cec6f5
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 6%

---

# 統合Experience PlatformWeb SDK

AEM as a Cloud ServiceとExperience Platformを統合する方法を説明します [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). この基本的なステップは、Adobe Analytics、Target などのAdobe Experience Cloud製品や、Real-time Customer Data Platform、Customer Journey Analytics、Journey Optimizerなどの最近の革新的な製品を統合するために不可欠です。

また、収集して送信する方法についても学習します [WKND — サンプルAdobe Experience Managerプロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ページビューデータ [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=ja).

この設定が完了したら、ソリッド基礎を実装しました。 また、次のようなアプリケーションを使用して、Experience Platformの実装を進める準備が整いました。 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html)、および [Adobe Journey Optimizer(AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja). 高度な実装は、Web および顧客データを標準化して、顧客エンゲージメントを促進するのに役立ちます。

## 前提条件

Experience PlatformWeb SDK を統合する場合は、次の操作が必要です。

In **AEM as aCloud Service**:

+ AEM as a Cloud Service 環境への AEM 管理者アクセス
+ デプロイメントマネージャーによる Cloud Manager へのアクセス
+ のクローンとデプロイ [WKND — サンプルAdobe Experience Managerプロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) をAEMas a Cloud Service環境に追加します。

In **Experience Platform**:

+ デフォルトの実稼動環境へのアクセス **Prod** サンドボックス。
+ アクセス先 **スキーマ** （データ管理）
+ アクセス先 **データセット** （データ管理）
+ アクセス先 **データストリーム** （データ収集）
+ アクセス先 **タグ** データ収集の下の（旧称：Launch）

必要な権限がない場合は、システム管理者が [Adobe Admin Console](https://adminconsole.adobe.com/) は必要な権限を付与できます。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM スキーマの作成 —Experience Platform

エクスペリエンスデータモデル (XDM) スキーマは、顧客体験データを標準化するのに役立ちます。 を収集するには、以下を実行します。 **WKND ページビュー** データ、XDM スキーマの作成、提供されたフィールドグループのAdobeの使用 `AEP Web SDK ExperienceEvent` web データ収集用。

小売、金融サービス、医療など、一連の参照データモデルに特化した汎用および業界があります。詳しくは、 [業界データモデルの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) を参照してください。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

XDM スキーマと関連する概念（フィールドグループ、タイプ、クラス、データタイプなど）については、 [XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

この [XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) は、XDM スキーマと関連する概念（フィールドグループ、タイプ、クラス、データ型など）について学ぶための優れたリソースです。 XDM データモデルの包括的な理解と、XDM スキーマを作成および管理して企業全体でデータを標準化する方法を提供します。 XDM スキーマを詳しく理解し、XDM スキーマがデータ収集および管理プロセスに与えるメリットを理解するために、このスキーマを調べます。

## データストリームを作成 —Experience Platform

Datastream は、収集したデータをどこに送信するかを Platform Edge Network に指示します。 例えば、Experience Platform、Analytics、Adobe Targetに送信できます。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

データストリームの概念と、データガバナンスや設定などの関連トピックについては、 [データストリームの概要](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=ja) ページ。

## タグプロパティを作成 —Experience Platform

Experience Platformでタグ（旧称 Launch）プロパティを作成し、Web SDK JavaScript ライブラリを WKND Web サイトに追加する方法について説明します。 新しく定義されたタグプロパティには、次のリソースが含まれています。

+ タグ拡張： [コア](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) および [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ データ要素：WKND サイトのAdobeクライアントデータレイヤーを使用して page-name、site-section、および host-name を抽出するカスタムコードタイプのデータ要素。 また、以前に作成した新しく作成された WKND XDM スキーマビルドインに準拠する XDM Object type データ要素 [XDM スキーマを作成](#create-xdm-schema---experience-platform) 手順
+ ルール：Adobeクライアントデータレイヤーをトリガーして WKND Web ページにアクセスするたびに、Platform Edge Network にデータを送信します `cmp:show` イベント。

を使用してタグライブラリを構築および公開する際に、 **公開フロー**&#x200B;を使用する場合、 **変更されたリソースをすべて追加** 」ボタンをクリックします。 個々のリソースを識別して選択する代わりに、データ要素、ルール、タグ拡張などのすべてのリソースを選択する場合。 また、開発フェーズでは、ライブラリを _開発_ 環境を検証し、 _ステージ_ または _実稼動_ 環境。

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>このビデオに示すデータ要素とルールイベントコードは、参照用に利用できます。 **下のアコーディオン要素を展開する**. ただし、Adobeクライアントデータレイヤーを使用していない場合は、以下のコードを変更する必要がありますが、データ要素を定義してルール定義で使用する概念は引き続き適用されます。


+++ データ要素とルールイベントコード

+ この `Page Name` データ要素コード。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ この `Site Section` データ要素コード。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ この `Host Name` データ要素コード。

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ この `all pages - on load` Rule-Event コード

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


この [タグの概要](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja) では、データ要素、ルール、拡張機能などの重要な概念に関する詳細な知識を提供します。

AEMコアコンポーネントとAdobeクライアントデータレイヤーの統合について詳しくは、 [AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用ガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja).

## タグプロパティをAEMに接続

AEMのAdobe IMSおよびAdobe起動設定を使用して、最近作成したタグプロパティをAEMにリンクする方法を説明します。 AEMas a Cloud Service環境が確立されると、Launch を含む、複数のAdobe IMSテクニカルアカウント設定が自動的に生成されます。 ただし、AEM 6.5 バージョンの場合は、手動で設定する必要があります。

タグプロパティをリンクした後、WKND サイトは、AdobeLaunch クラウドサービスの設定を使用して、タグプロパティの JavaScript ライブラリを Web ページに読み込むことができます。

### WKND でのタグプロパティの読み込みを確認

Adobe Experience Platform Debugger の使用 [クロム](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) または [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 拡張機能で、タグプロパティが WKND ページに読み込まれているかどうかを確認します。 君は確かめる事が出来る

+ タグプロパティの詳細（拡張機能、バージョン、名前など）。
+ Platform Web SDK ライブラリのバージョン、Datastream ID
+ 一部としての XDM オブジェクト `events` Experience PlatformWeb SDK の属性

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## データセットを作成 —Experience Platform

Web SDK を使用して収集されたページビューデータは、データセットとしてExperience Platformデータレイクに保存されます。 データセットは、スキーマに従うデータベーステーブルなど、データの集まりのストレージと管理の構成体です。 データセットを作成し、以前に作成したデータストリームを設定して、データをExperience Platformに送信する方法を説明します。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

この [データセットの概要](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=ja) の概念、設定、その他の取り込み機能に関する詳細情報を提供します。


## Experience Platform内の WKND ページビューデータ

AEM（特に WKND サイト）で Web SDK を設定したら、サイトページを移動してトラフィックを生成する番です。 次に、ページビューデータがExperience Platformデータセットに取り込まれていることを確認します。 データセット UI 内では、合計レコード数、サイズ、取り込まれたバッチなどの様々な詳細が、視覚に訴える棒グラフと共に表示されます。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 概要

お疲れ様でした。Web サイトからデータを収集および取り込むための、Experience PlatformWeb SDK を使用したAEMの設定が完了している。 この基盤を使用すると、Analytics、Target、Customer Journey Analytics(CJA) などの製品を強化および統合して、顧客に合わせてパーソナライズされた豊富なエクスペリエンスを作成できるようになります。 学び続け、Adobe Experience Cloudの可能性を最大限に引き出すために探索し続けます。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## その他のリソース

+ [コアコンポーネントでの Adobe Client Data Layer の使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)
+ [Experience Platformデータ収集タグとAEMの統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK と Edge Network の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [データ収集チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
