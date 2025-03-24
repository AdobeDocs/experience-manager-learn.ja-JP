---
title: Platform Web SDK を使用した AEM Sites と Adobe Analytics の統合
description: 最新の Platform Web SDK アプローチを使用して、AEM Sites と Adobe Analytics を統合します。
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 100%

---

# Platform Web SDK を使用した AEM Sites と Adobe Analytics の統合

Platform Web SDK を使用して Adobe Experience Manager（AEM）と Adobe Analytics を統合する方法に関する&#x200B;**最新のアプローチ**&#x200B;を説明します。この包括的なチュートリアルでは、[WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ページビュー数と CTA クリック数のデータをシームレスに収集するプロセスを説明します。様々な指標やディメンションを参照できる Adobe Analysis Workspace で収集したデータを視覚化することで、有益なインサイトを得ることができます。また、Platform データセットを参照してデータを検証および分析することもできます。AEM と Adobe Analytics の機能をデータ駆動型の意思決定に活用するこのジャーニーに参加してください。

## 概要

ユーザーの行動に関するインサイトを得ることは、すべてのマーケティングチームにとって重要な目標です。ユーザーがコンテンツとやり取りする方法を理解することで、チームは情報に基づいた意思決定を行い、戦略を最適化し、より良い結果を導くことができます。架空の組織である WKND マーケティングチームは、この目標を達成するために、web サイトに Adobe Analytics を実装することを目指しています。主な目的は、ページビュー数とホームページコールトゥアクション（CTA）クリック数という 2 つの主要な指標に関するデータを収集することです。

チームは、ページビュー数をトラッキングすることで、ユーザーから最も注目を集めているページを分析できます。また、ホームページでの CTA クリック数のトラッキングは、チームのコールトゥアクション要素の効果に関する貴重なインサイトを提供します。このデータは、ユーザーの反応を呼んでいる CTA を明らかにし、調整が必要な CTA を示し、ユーザーエンゲージメントを強化してコンバージョンを促進する新しい機会を明らかにする可能性があります。


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## 前提条件

Platform Web SDK を使用して Adobe Analytics を統合するには、次のことが必要です。

**[Experience Platform Web SDK の統合](./web-sdk.md)**&#x200B;チュートリアルの設定手順を完了している。

**AEM as Cloud Service** で：

+ [AEM as a Cloud Service 環境への AEM 管理者アクセス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=ja)
+ デプロイメントマネージャーによる Cloud Manager へのアクセス
+ [WKND - サンプル Adobe Experience Manager プロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)のクローンを作成して、AEM as a Cloud Service 環境にデプロイします。

**Adobe Analytics** で：

+ **レポートスイート**&#x200B;の作成へのアクセス
+ **Analysis Workspace** を作成するためのアクセス

**Experience Platform** で：

+ デフォルトの実稼動環境 **Prod** サンドボックスへアクセスします。
+ データ管理の&#x200B;**スキーマ**&#x200B;にアクセスします。
+ データ管理の&#x200B;**データセット**&#x200B;にアクセスします。
+ データ管理の&#x200B;**データストリーム**&#x200B;にアクセスします。
+ データ収集の&#x200B;**タグ**&#x200B;にアクセスします。

必要な権限がない場合は、[Adobe Admin Console](https://adminconsole.adobe.com/) を使用して、システム管理者が必要な権限を付与できます。

Platform Web SDK を使用したAEMと Analytics の統合プロセスを詳しく説明する前に、[Experience Platform Web SDK の統合](./web-sdk.md)チュートリアルで確立された&#x200B;_基本的なコンポーネントと主要な要素_&#x200B;をまとめてみましょう。これは統合に向けた強固な基盤となります。

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

XDM スキーマ、Datastream、データセット、タグプロパティおよび AEM とタグプロパティの接続の概要を確認した後、統合ジャーニーを始めましょう。

## 分析ソリューションデザインリファレンス（SDR）ドキュメントの定義

実装プロセスの一環として、ソリューションデザインリファレンス（SDR）ドキュメントを作成することをお勧めします。このドキュメントは、ビジネス要件の定義と効果的なデータ収集戦略を設計する上でのブループリントとして重要な役割を果たします。

SDR ドキュメントは、実装計画の包括的な概要を提供し、すべての関係者が足並みをそろえ、プロジェクトの目的と範囲を理解できるようにします。


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

SDR ドキュメントに含める必要のある概念と様々な要素について詳しくは、[ソリューションデザインリファレンス（SDR）ドキュメントの作成と管理](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html?lang=ja)にアクセスしてください。サンプルの Excel テンプレートもダウンロードできますが、WKND 固有のバージョンも[こちら](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx)から利用できます。

## Analytics の設定 - レポートスイート、Analysis Workspace

最初の手順は、Adobe Analytics、具体的にはコンバージョン変数（または eVar）と成功イベントを含むレポートスイートを設定することです。コンバージョン変数は、原因と結果の測定に使用します。成功イベントは、アクションのトラックに使用します。

このチュートリアルでは、`eVar5, eVar6, and eVar7` は _WKND ページ名、WKND CTA ID、WKND CTA 名_&#x200B;をそれぞれトラックし、`event7` を使用して _WKND CTA クリックイベント_&#x200B;をトラックします。

収集したデータを分析し、インサイトを収集し、それらのインサイトを他のユーザーと共有するには、Analysis Workspace のプロジェクトを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Analytics の設定と概念について詳しくは、次のリソースを参照することを強くお勧めします。

+ [レポートスイート](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=ja)
+ [コンバージョン変数](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=ja)
+ [成功イベント](https://experienceleague.adobe.com/ja/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=ja)

## データストリームの更新 - Analytics サービスの追加

データストリームは、収集したデータの送信先を Platform Edge Network に指示します。[前のチュートリアル](./web-sdk.md)では、データを Experience Platform に送信するようにデータストリームが設定されています。このデータストリームは、[上記](#setup-analytics---report-suite-analysis-workspace)の手順で設定された Analytics レポートスイートにデータを送信するように更新されます。

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## XDM スキーマの作成

エクスペリエンスデータモデル（XDM）スキーマを使用すると、収集したデータを標準化できます。[前のチュートリアル](./web-sdk.md)では、`AEP Web SDK ExperienceEvent` フィールドグループを含む XDM スキーマが作成されます。また、この XDM スキーマを使用して、収集したデータを Experience Platform に保存するためのデータセットを作成します。

ただし、XDM スキーマには、eVar やイベントデータを送信する Adobe Analytics 固有のフィールドグループがありません。プラットフォームに eVar やイベントデータが保存されないように、既存のスキーマを更新する代わりに新しい XDM スキーマが作成されます。

新しく作成された XDM スキーマには、`AEP Web SDK ExperienceEvent` および `Adobe Analytics ExperienceEvent Full Extension` フィールドグループがあります。

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## タグプロパティの更新

[前のチュートリアル](./web-sdk.md)では、タグプロパティが作成され、ページビューデータの収集、マッピング、送信を行うためのデータ要素とルールが含まれています。以下を強化する必要があります。

+ ページ名のマッピング先 `eVar5`
+ **pageview** Analytics 呼び出し（またはビーコンを送信）のトリガー
+ Adobe Client Data Layer を使用した CTA データの収集
+ CTA ID と名前をそれぞれ `eVar6` および `eVar7` にマッピングします。また、CTA のクリック数は `event7` までカウントされます。
+ **リンククリック** Analytics 呼び出し（またはビーコンを送信）のトリガー


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>このビデオに示されているデータ要素とルールイベントコードは、参照用に利用できます。**下のアコーディオン要素を展開してください**。ただし、Adobe Client Data Layer を使用していない場合は、以下のコードを変更する必要がありますが、データ要素を定義してルール定義で使用するという概念は引き続き適用されます。

+++ データ要素とルールイベントコード

+ `Component ID` データ要素コード。

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ `Component Name` データ要素コード。

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ `all pages - on load` **ルール条件**&#x200B;コード

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ `home page - cta click` ルールイベントコード&#x200B;****

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ `home page - cta click` **ルール条件**&#x200B;コード

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

AEM Core Components と Adobe Client Data Layer の統合に関する追加情報については、[AEM コアコンポーネントでの Adobe Client Data Layer の使用ガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)を参照してください。


>[!INFO]
>
>ソリューションデザインリファレンス（SDR）ドキュメントの「**変数マップ**」タブのプロパティの詳細を包括的に理解するには、[こちら](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx)から完全な WKND 固有のバージョンにアクセスしてダウンロードしてください。



## WKND で更新されたタグプロパティの検証

更新されたタグプロパティが WKND サイトページ上でビルド、公開され、正しく機能していることを確認します。Google Chrome web ブラウザーの [Adobe Experience Platform Debugger 拡張機能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)を使用します。

+ タグプロパティが最新バージョンであることを確認するには、ビルド日を確認します。

+ ページビュー数とホームページ CTA クリック数の両方の XDM イベントデータを確認するには、拡張機能内の Experience Platform Web SDK メニューオプションを使用します。

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Web トラフィックのシミュレート - Selenium の自動処理

テスト目的で意味のある量のトラフィックを生成するために、Selenium 自動処理スクリプトが開発されています。このカスタムスクリプトは、ページビュー数や CTA クリック数など、WKND web サイトとのユーザーインタラクションをシミュレートします。

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## データセットの検証 - WKND ページビュー数、CTA データ

データセットは、スキーマに従うデータベーステーブルのように、データの集まりのためのストレージと管理の構成体です。[以前のチュートリアル](./web-sdk.md)で作成したデータセットは、ページビュー数と CTA クリック数のデータが Experience Platform データセットに取り込まれていることを確認するために再利用されます。データセット UI には、総レコード数、サイズ、取り込まれたバッチ数など、様々な詳細が視覚的にわかりやすい棒グラフと共に表示されます。

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND ページビュー数、CTA データビジュアライゼーション

Analysis Workspace は、柔軟かつインタラクティブな方法でデータを探索および視覚化できる Adobe Analytics 内の強力なツールです。カスタムレポートの作成、高度なセグメント化の実行、様々なデータビジュアライゼーションの適用を行うためのドラッグ＆ドロップインターフェイスを提供します。

[Analytics の設定](#setup-analytics---report-suite-analysis-workspace)手順で作成した Analysis Workspace プロジェクトを再度開きます。「**トップページ**」セクションでは、訪問数、ユニーク訪問者数、エントリ数、バウンス率などの様々な指標を調べます。WKND ページとホームページ CTA のパフォーマンスを評価するには、WKND 固有のディメンション（WKND ページ名、WKND CTA 名）と指標（WKND CTA クリックイベント）をドラッグ＆ドロップします。これらのインサイトは、マーケターが効果的な CTA を理解し、ビジネス目標に合わせてデータ駆動型の意思決定を行うのに役立ちます。

ユーザージャーニーを視覚化するには、**WKND ページ名**&#x200B;から始めて様々なパスに拡張するフロービジュアライゼーションを使用します。

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## 概要

お疲れ様でした。 Platform Web SDK を使用してページビュー数と CTA クリック数のデータを収集、分析するための AEM と Adobe Analytics の設定が完了しました。

Adobe Analytics の実装は、マーケティングチームがユーザーの行動に関するインサイトを得て、情報に基づいた意思決定を行い、コンテンツを最適化し、データ駆動型の意思決定を行ううえできわめて重要です。

推奨手順を実行し、ソリューションデザインリファレンス（SDR）ドキュメントなどの提供されたリソースを使用し、Analytics の主要な概念を理解することで、マーケターは効果的にデータを収集して分析できます。

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>個別の設定手順のビデオではなく、統合プロセス全体に対応する&#x200B;**エンドツーエンド**&#x200B;のビデオを希望する場合は、[こちら](https://video.tv.adobe.com/v/3419889/)をクリックしてアクセスできます。


## その他のリソース

+ [Experience Platform Web SDK の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html?lang=ja)
+ [コアコンポーネントでの Adobe Client Data Layer の使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)
+ [Experience Platform データ収集タグと AEM の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=ja)
+ [Adobe Experience Platform Web SDK と Edge Network の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=ja)
+ [データ収集チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=ja)
+ [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja)
