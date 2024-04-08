---
title: AEM Sites と Experience Platform Web SDK の統合
description: AEM Sites as a Cloud Service と Experience Platform Web SDK を統合する方法を説明します。この基本的なステップは、Adobe Analytics、Target などの Adobe Experience Cloud 製品や、Real-time Customer Data Platform、Customer Journey Analytics、Journey Optimizer などの最近の革新的な製品を統合するために不可欠です。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1360
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 90%

---

# AEM Sites と Experience Platform Web SDK の統合

AEM as a Cloud Service と Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) を統合する方法を説明します。この基本的なステップは、Adobe Analytics、Target などの Adobe Experience Cloud 製品や、Real-time Customer Data Platform、Customer Journey Analytics、Journey Optimizer などの最近の革新的な製品を統合するために不可欠です。

また、[Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=ja) で [WKND - サンプル Adobe Experience Manager プロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)のページビュー データを収集して送信する方法も説明します。

このセットアップを完了すると、強固な基盤が実装されたことになります。また、[Real-time Customer Data Platform（Real-Time CDP）](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ja)、[Customer Journey Analytics（CJA）](https://experienceleague.adobe.com/docs/customer-journey-analytics.html?lang=ja)、[Adobe Journey Optimizer（AJO）](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja)などのアプリケーションを使用して Experience Platform の実装を進める準備も整います。高度な実装は、web と顧客のデータを標準化することで、顧客エンゲージメントの向上に役立ちます。

## 前提条件

Experience Platform Web SDK を統合するには、次の操作が必要です。

**AEM as Cloud Service** で：

+ AEM as a Cloud Service 環境への AEM 管理者アクセス
+ デプロイメントマネージャーによる Cloud Manager へのアクセス
+ [WKND - サンプル Adobe Experience Manager プロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)のクローンを作成して、AEM as a Cloud Service 環境にデプロイします。

**Experience Platform** で：

+ デフォルトの実稼動環境 **Prod** サンドボックスへアクセスします。
+ データ管理の&#x200B;**スキーマ**&#x200B;にアクセスします。
+ データ管理の&#x200B;**データセット**&#x200B;にアクセスします。
+ データ管理の&#x200B;**データストリーム**&#x200B;にアクセスします。
+ アクセス先 **タグ** （データ収集）

必要な権限がない場合は、[Adobe Admin Console](https://adminconsole.adobe.com/) を使用して、システム管理者が必要な権限を付与できます。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM スキーマの作成 - Experience Platform

エクスペリエンスデータモデル（XDM）スキーマを使用すると、カスタマーエクスペリエンスのデータを標準化できます。**WKND ページビュー**&#x200B;のデータを収集するには、XDM スキーマを作成し、アドビが提供するフィールド グループ `AEP Web SDK ExperienceEvent` を web データ収集に使用します。

小売、金融機関、ヘルスケアなどの汎用および業界固有の参照データモデルのスイートがあります。詳しくは、[業界データモデルの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html?lang=ja)を参照してください。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

[XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)で、XDM スキーマと関連概念（フィールド グループ、タイプ、クラス、データ型など）について説明します。

[XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)は、XDM スキーマと関連概念（フィールドグループ、タイプ、クラス、データ型など）について知るための優れたリソースです。XDM データモデルの包括的な理解と、XDM スキーマを作成および管理して企業全体でデータを標準化する方法を提供します。XDM スキーマを詳しく理解し、XDM スキーマがデータの収集プロセスと管理プロセスに与えるメリットを確認します。

## データストリームの作成 - Experience Platform

データストリームは、収集したデータの送信先を Platform Edge Network に指示します。例えば、Experience Platform、Analytics、Adobe Target に送信されることがあります。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

[データストリームの概要](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=ja)ページにアクセスして、データ ストリームの概念と、データのガバナンスや構成などの関連トピックを理解します。

## タグプロパティの作成 - Experience Platform

Web SDK JavaScript ライブラリを WKND Web サイトに追加するために、Experience Platformでタグプロパティを作成する方法について説明します。 新しく定義されたタグプロパティには、次のリソースが含まれています。

+ タグ拡張：[コア](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension)および [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ データ要素：WKND サイトの Adobe Client Data Layer を使用して page-name、site-section、host-name を抽出するカスタムコードタイプのデータ要素。および、新しく作成した WKND XDM スキーマビルドインに準拠する XDM オブジェクトタイプのデータ要素（以前に「[XDM スキーマを作成](#create-xdm-schema---experience-platform)」手順で作成）
+ ルール：Adobe Client Data Layer がトリガーする `cmp:show` イベントを使用して、WKND web ページにアクセスするたびに、プラットフォーム エッジ ネットワークにデータを送信します。

**公開フロー**&#x200B;を使用してタグライブラリをビルドおよび公開する際に、「**変更されたすべてのリソースを追加**」ボタンを使用できます。個々のリソースを識別して選択する代わりに、データ要素、ルール、タグ拡張機能などのすべてのリソースを選択します。また、開発フェーズでは、ライブラリを&#x200B;_開発環境_&#x200B;のみに公開し、検証して&#x200B;_ステージング_&#x200B;環境または&#x200B;_実稼動_&#x200B;環境に昇格することもできます。

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>ビデオに示されているデータ要素とルールイベントコードは参照用に利用できます。**以下のアコーディオン要素を展開**&#x200B;してください。ただし、Adobe Client Data Layer を使用していない場合は、以下のコードを変更する必要がありますが、データ要素を定義してルール定義で使用するという概念は引き続き適用されます。


+++ データ要素とルールイベントコード

+ `Page Name` データ要素コード。

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ `Site Section` データ要素コード。

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

+ `Host Name` データ要素コード。

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ `all pages - on load` ルールイベントコード

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


[タグの概要](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja) では、データ要素、ルール、エクステンションなどの重要な概念に関する詳細な知識を提供します。

AEM Core Components と Adobe Client Data Layer の統合に関する追加情報については、[AEM コアコンポーネントでの Adobe Client Data Layer の使用ガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)を参照してください。

## タグプロパティを AEM に接続

AEMのAdobe Experience Platform設定でAdobe IMSとタグを使用して、最近作成したタグプロパティをAEMにリンクする方法を説明します。 AEMas a Cloud Service環境が確立されると、タグを含む複数のAdobe IMSテクニカルアカウント設定が自動的に生成されます。 ただし、AEM 6.5 バージョンの場合は、手動で設定する必要があります。

タグプロパティをリンクすると、WKND サイトは、 Adobe Experience Platform Cloud Service 設定のタグを使用して、タグプロパティの JavaScript ライブラリを Web ページに読み込むことができます。

### WKND へのタグプロパティの読み込みを検証

使用Adobe Experience Platform Debugger [クロム](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 拡張機能で、タグプロパティが WKND ページに読み込まれているかどうかを確認します。 次を確認します。

+ タグプロパティの詳細（拡張機能、バージョン、名前など）。
+ Platform Web SDK ライブラリのバージョン、データストリーム ID
+ Experience Platform Web SDK の一部の `events` 属性としての XDM オブジェクト

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## データセットの作成 - Experience Platform

Web SDK を使用して収集したページビューのデータは、データセットとして Experience Platform データレイクに保存されます。データセットは、スキーマに従うデータベーステーブルのように、データの集まりのためのストレージと管理の構成体です。データセットを作成して、以前に作成したデータストリームを設定し、データを Experience Platform に送信する方法を説明します。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

[データセットの概要](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html?lang=ja)では、概念、設定、その他の取り込み機能に関して追加情報を提供します。


## Experience Platform 内の WKND ページビューデータ

AEM（特に WKND サイト）で Web SDK を設定したら、サイトページを移動してトラフィックを生成します。次に、ページビューのデータが Experience Platform Dataset に取り込まれていることを確認します。データセット UI には、総レコード数、サイズ、取り込まれたバッチ数など、様々な詳細が視覚的にわかりやすい棒グラフと共に表示されます。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 概要

お疲れ様でした。Web サイトからデータを収集して取り込むための、Experience Platform Web SDK を使用した AEM の設定が完了しました。この基盤を使用すると、Analytics、Target、Customer Journey Analytics（CJA）などの製品を強化および統合して、顧客に合わせてパーソナライズされた豊富なエクスペリエンスを提供できる可能性を追求できるようになります。Adobe Experience Cloud の可能性を最大限に引き出すため、学習と探索を続けてください。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>個々のセットアップ手順のビデオではなく、統合プロセス全体をカバーする&#x200B;**エンドツーエンドのビデオ**&#x200B;を希望する場合は、[こちら](https://video.tv.adobe.com/v/3418905/)をクリックしてアクセスできます。

## その他のリソース

+ [コアコンポーネントでの Adobe Client Data Layer の使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)
+ [Experience Platform データ収集タグと AEM の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=ja)
+ [Adobe Experience Platform Web SDK と Edge Network の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=ja)
+ [データ収集チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=ja)
+ [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja)
