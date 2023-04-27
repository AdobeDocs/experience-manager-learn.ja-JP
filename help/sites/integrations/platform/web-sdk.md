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
source-git-commit: 593ef5767a5f2321c689e391f9c9019de7c94672
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 3%

---


# 統合Experience PlatformWeb SDK

AEM as a Cloud ServiceとExperience Platformを統合する方法を説明します [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). この基本的なステップは、Adobe Analytics、Target などのAdobe Experience Cloud製品や、Real-time Customer Data Platform、Customer Journey Analytics、Journey Optimizerなどの最近の革新的な製品を統合するために不可欠です。

また、収集して送信する方法についても学習します [WKND — サンプルAdobe Experience Managerプロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ページビューデータ [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=ja).

このセットアップが完了したら、次に、次のようなExperience Platformおよび関連アプリケーションを実装できます。 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) および [Adobe Journey Optimizer(AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja). ウェブ及び顧客データを標準化し、より良い顧客エンゲージメントを促進する。

## 前提条件

Experience PlatformWeb SDK を統合する場合は、次の操作が必要です。

In **AEM as aCloud Service**:

+ AEM AEM as a Cloud Service環境への管理者アクセス
+ Cloud Manager への Deployment Manager アクセス
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


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

XDM スキーマと関連する概念（フィールドグループ、タイプ、クラス、データタイプなど）については、 [XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

この [XDM システムの概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) は、XDM スキーマと関連する概念（フィールドグループ、タイプ、クラス、データ型など）について学ぶための優れたリソースです。 XDM データモデルの包括的な理解と、XDM スキーマを作成および管理して企業全体でデータを標準化する方法を提供します。 XDM スキーマを詳しく理解し、XDM スキーマがデータ収集および管理プロセスに与えるメリットを理解するために、このスキーマを調べます。

## DataStream の作成 —Experience Platform

DataStream は、収集されたデータをどこに送信するかを Platform Edge Network に指示します。 例えば、Experience Platform、Analytics、Adobe Targetに送信できます。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

データストリームの概念と、データガバナンスや設定などの関連トピックについては、 [データストリームの概要](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html?lang=ja) ページ。

## タグプロパティを作成 —Experience Platform

Experience Platformでタグ（旧称 Launch）プロパティを作成し、Web SDK JavaScript ライブラリを WKND Web サイトに追加する方法について説明します。 新しく定義されたタグプロパティには、次のリソースが含まれています。

+ タグ拡張： [コア](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) および [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ データ要素：WKND サイトのAdobeクライアントデータレイヤーを使用して page-name、site-section、および host-name を抽出するカスタムコードタイプのデータ要素。 また、以前に作成した新しく作成された WKND XDM スキーマビルドインに準拠する XDM Object type データ要素 [XDM スキーマを作成](#create-xdm-schema---experience-platform) 手順
+ ルール：Adobeクライアントデータレイヤーをトリガーして WKND Web ページにアクセスするたびに、Platform Edge Network にデータを送信します `cmp:show` イベント。


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)

この [タグの概要](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) では、データ要素、ルール、拡張機能などの重要な概念に関する詳細な知識を提供します。

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

お疲れ様でした。Web サイトからデータを収集および取り込むための、Adobe Experience Platform(Experience Platform)Web SDK を使用したAEMの設定が完了している。 この基盤を使用すると、Analytics、Target、Customer Journey Analytics(CJA) などの製品を強化および統合して、顧客に合わせてパーソナライズされた豊富なエクスペリエンスを作成できるようになります。 学び続け、Adobe Experience Cloudの可能性を最大限に引き出すために探索し続けます。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)
