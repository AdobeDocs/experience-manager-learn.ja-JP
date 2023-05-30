---
title: Platform Web SDK を使用したAdobe Analyticsの統合
description: Platform Web SDK を使用してAdobe Experience Manager(AEM) とAdobe Analyticsを統合する方法に関する最新のアプローチについて説明します。 このチュートリアルでは、ページビューと CTA クリックデータを収集し、Adobe Analytics Workspace でデータインサイトを得る手順を説明します。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
source-git-commit: 3831c6ed1467018c9f5bf15aa9f6b8ee78034c02
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 4%

---


# Platform Web SDK を使用したAdobe Analyticsの統合

詳しくは、 **現代的アプローチ** Platform Web SDK を使用してAdobe Experience Manager(AEM) とAdobe Analyticsを統合する方法について説明します。 この包括的なチュートリアルでは、シームレスに収集するプロセスをガイドします [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ページビューおよび CTA クリックデータ。 様々な指標およびディメンションを参照できるAdobeAnalysis Workspaceで、収集したデータを視覚化して、有益なインサイトを得ます。 また、Platform データセットを調べて、データを検証および分析します。 AEMとAdobe Analyticsの力を活かし、データ主導型の意思決定に取り組んでください。

## 概要

ユーザーの行動に関するインサイトを得ることは、すべてのマーケティングチームにとって重要な目標です。 ユーザーがコンテンツとどのようにやり取りするかを理解することで、チームは十分な情報に基づく意思決定を行い、戦略を最適化し、より良い結果を引き出すことができます。 架空のエンティティである WKND マーケティングチームは、この目標を達成するために、Web サイトにAdobe Analyticsを実装することを目指しています。 主な目的は、次の 2 つの主要指標に関するデータを収集することです。ページビュー数とホームページのコールトゥアクション (CTA) クリック数。

チームは、ページビュー数を追跡することで、ユーザーから最も注目を集めているページを分析できます。 また、ホームページでの CTA クリック数の追跡は、チームのコールトゥアクション要素の効果に関する貴重なインサイトを提供します。 このデータは、どの CTA がユーザーの反応を呼んでいるかを明らかにし、調整が必要な CTA を示し、ユーザーエンゲージメントを強化しコンバージョンを促進する新しい機会を明らかにする可能性があります。


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## 前提条件

Platform Web SDK を使用してAdobe Analyticsを統合する場合、次の操作が必要です。

次の設定手順が完了しました： **[統合Experience PlatformWeb SDK](./web-sdk.md)** チュートリアル

In **AEM as aCloud Service**:

+ [AEM as a Cloud Service 環境への AEM 管理者アクセス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=ja)
+ デプロイメントマネージャーによる Cloud Manager へのアクセス
+ のクローンとデプロイ [WKND — サンプルAdobe Experience Managerプロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) をAEMas a Cloud Service環境に追加します。

In **Adobe Analytics**:

+ 作成へのアクセス **レポートスイート**
+ 作成へのアクセス **Analysis Workspace**

In **Experience Platform**:

+ デフォルトの実稼動環境へのアクセス **Prod** サンドボックス。
+ アクセス先 **スキーマ** （データ管理）
+ アクセス先 **データセット** （データ管理）
+ アクセス先 **データストリーム** （データ収集）
+ アクセス先 **タグ** データ収集の下の（旧称：Launch）

必要な権限がない場合は、システム管理者が [Adobe Admin Console](https://adminconsole.adobe.com/) は必要な権限を付与できます。

Platform Web SDK を使用したAEMと Analytics の統合プロセスについて説明する前に、 _基本的なコンポーネントと主要な要素をまとめる_ それは [統合Experience PlatformWeb SDK](./web-sdk.md) チュートリアル 統合の基盤となります。

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

XDM スキーマ、Datastream、データセット、タグプロパティ、およびAEMとタグプロパティの接続の概要を説明した後、統合ジャーニーに着手しましょう。

## Analytics ソリューションデザインリファレンス (SDR) ドキュメントの定義

実装プロセスの一環として、ソリューションデザインリファレンス (SDR) ドキュメントを作成することをお勧めします。 このドキュメントは、ビジネス要件の定義と効果的なデータ収集戦略の設計のためのブループリントとして重要な役割を果たします。

SDR ドキュメントは、実装計画の包括的な概要を提供し、すべての関係者が一致し、プロジェクトの目的と範囲を理解できるようにします。


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

SDR ドキュメントに含める必要のある概念と様々な要素の詳細については、 [ソリューションデザインリファレンス (SDR) ドキュメントの作成と管理](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). サンプルの Excel テンプレートもダウンロードできますが、WKND 固有のバージョンも利用できます [ここ](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Analytics の設定 — レポートスイート、Analysis Workspace

最初の手順は、Adobe Analyticsを設定することです。特に、コンバージョン変数 ( またはeVar) と成功イベントを使用してレポートスイートを設定します。 コンバージョン変数は、原因と効果の測定に使用されます。 成功イベントは、アクションの追跡に使用されます。

このチュートリアルでは、  `eVar5, eVar6, and eVar7` track  _WKND ページ名、WKND CTA ID、WKND CTA 名_ それぞれおよび `event7` を追跡するために使用します。  _WKND CTA Click イベント_.

収集したデータを分析、収集し、それらのインサイトを他のユーザーと共有するには、Analysis Workspaceのプロジェクトが作成されます。

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Analytics の設定と概念について詳しくは、次のリソースを強くお勧めします。

+ [レポートスイート](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=ja)
+ [コンバージョン変数](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [成功イベント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=ja)

## データストリームの更新 — Analytics サービスの追加

Datastream は、収集したデータをどこに送信するかを Platform Edge Network に指示します。 内 [前のチュートリアル](./web-sdk.md)を指定しない場合、データをExperience Platformに送信するようにデータストリームが設定されます。 この Datastream は更新され、 [上](#setup-analytics---report-suite-analysis-workspace) 手順

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## XDM スキーマを作成

Experience Data Model(XDM) スキーマは、収集したデータを標準化するのに役立ちます。 内 [前のチュートリアル](./web-sdk.md)（を使用した XDM スキーマ） `AEP Web SDK ExperienceEvent` フィールドグループが作成されます。 また、この XDM スキーマを使用して、収集したデータをデータセットに保存するデータセットをExperience Platformします。

ただし、この XDM スキーマには、eVarやイベントデータを送信するAdobe Analytics固有のフィールドグループはありません。 プラットフォームにeVarやイベントデータが保存されないように、既存のスキーマを更新する代わりに新しい XDM スキーマが作成されます。

新しく作成された XDM スキーマには、 `AEP Web SDK ExperienceEvent` および `Adobe Analytics ExperienceEvent Full Extension` フィールドグループを使用します。

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## タグプロパティを更新

内 [前のチュートリアル](./web-sdk.md)タグプロパティが作成され、データ要素と、ページビューデータの収集、マッピング、送信をおこなうためのルールを持ちます。 次の目的で拡張する必要があります。

+ ページ名のマッピング先 `eVar5`
+ トリガー **pageview** Analytics 呼び出し（またはビーコンを送信）
+ Adobeクライアントデータレイヤーを使用した CTA データの収集
+ CTA ID と名前のマッピング先 `eVar6` および `eVar7` それぞれ また、CTA のクリック数は、 `event7`
+ トリガー **リンククリック** Analytics 呼び出し（またはビーコンを送信）


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>このビデオに示すデータ要素とルールイベントコードは、参照用に利用できます。 **下のアコーディオン要素を展開する**. ただし、Adobeクライアントデータレイヤーを使用していない場合は、以下のコードを変更する必要がありますが、データ要素を定義してルール定義で使用する概念は引き続き適用されます。

+++ データ要素とルールイベントコード

+ この `Component ID` データ要素コード。

   ```javascript
   if(event && event.path && event.path.includes('.')) {    
       // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
       return event.path.split('.')[1];
   }else {
       //return dummy ID
       return "WKND-CTA-ID";
   }
   ```

+ この `Component Name` データ要素コード。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
       return event.component['dc:title'];
   }else {
       //return dummy ID
       return "WKND-CTA-Name";    
   }    
   ```

+ この `all pages - on load` **Rule-Condition** コード

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
       return true;
   }else{
       return false;
   }    
   ```

+ この `home page - cta click` **Rule-Event** コード

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

+ この `home page - cta click` **Rule-Condition** コード

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

AEMコアコンポーネントとAdobeクライアントデータレイヤーの統合について詳しくは、 [AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用ガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja).


>[!INFO]
>
>の包括的な理解 **変数マップ** ソリューションデザインリファレンス (SDR) ドキュメントのタブプロパティの詳細に、完成した WKND 固有のバージョンにアクセスしてダウンロードします。 [ここ](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## WKND で更新されたタグプロパティを確認

WKND サイトページで、更新されたタグプロパティが構築、公開され、正しく機能していることを確認する。 Google Chrome Web ブラウザーの使用 [Adobe Experience Platform Debugger 拡張機能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ タグプロパティが最新バージョンであることを確認するには、ビルド日を確認してください。

+ PageView と HomePage CTA Click の両方の XDM イベントデータを検証するには、拡張機能内の「 Web SDK をExperience Platform」メニューオプションを使用します。

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Web トラフィックをシミュレート — Selenium の自動化

テスト目的で有意な量のトラフィックを生成するために、Selenium 自動化スクリプトを開発しました。 このカスタムスクリプトは、ページビューや CTA のクリックなど、WKND Web サイトでのユーザーインタラクションをシミュレートします。

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## データセットの検証 — WKND ページビュー、CTA データ

データセットは、スキーマに従うデータベーステーブルなど、データの集まりのストレージと管理の構成体です。 データセットは [前のチュートリアル](./web-sdk.md) は、ページビューと CTA クリックデータがExperience Platformデータセットに取り込まれることを確認するために再利用されます。 データセット UI 内では、合計レコード数、サイズ、取り込まれたバッチなどの様々な詳細が、視覚に訴える棒グラフと共に表示されます。

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND ページビュー、CTA データビジュアライゼーション

Analysis Workspaceは、柔軟でインタラクティブな方法でデータを調査および視覚化できる、Adobe Analytics内の強力なツールです。 カスタムレポートの作成、高度なセグメント化の実行、様々なデータビジュアライゼーションの適用をおこなうためのドラッグ&amp;ドロップインターフェイスが提供されます。

で作成したAnalysis Workspaceプロジェクトを再度開きます。 [Analytics を設定](#setup-analytics---report-suite-analysis-workspace) 手順 内 **トップページ** セクションでは、訪問回数、個別訪問者数、入口数、バウンス率など、様々な指標を調べます。 WKND ページとホームページ CTA のパフォーマンスを評価するには、WKND 固有のディメンション（WKND ページ名、WKND CTA 名）と指標（WKND CTA クリックイベント）をドラッグ&amp;ドロップします。 これらのインサイトは、マーケターが効果的な CTA を把握し、ビジネス目標に沿ってデータ主導型の意思決定をおこなうのに役立ちます。

ユーザージャーニーを視覚化するには、以下を使用して、フロービジュアライゼーションを使用します。 **WKND ページ名** 様々な経路に展開する

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## 概要

お疲れ様でした。Platform Web SDK を使用してAEMとAdobe Analyticsの設定を完了し、ページビューと CTA クリックデータを収集、分析しました。

Adobe Analyticsの実装は、マーケティングチームがユーザーの行動に関するインサイトを得、十分な情報に基づいた意思決定を行い、コンテンツを最適化し、データに基づく意思決定をおこなううえで非常に重要です。

ソリューションデザインリファレンス (SDR) ドキュメントなどの推奨手順を実装し、提供されるリソースを使用し、Analytics の主要な概念を理解することで、マーケターはデータを効果的に収集および分析できます。

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>必要に応じて **エンドツーエンドビデオ** これは、個々のセットアップ手順のビデオではなく、統合プロセス全体に対応しています。 [ここ](https://video.tv.adobe.com/v/3419889/) にアクセスします。


## その他のリソース

+ [統合Experience PlatformWeb SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [コアコンポーネントでの Adobe Client Data Layer の使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)
+ [Experience Platformデータ収集タグとAEMの統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK と Edge Network の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [データ収集チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
