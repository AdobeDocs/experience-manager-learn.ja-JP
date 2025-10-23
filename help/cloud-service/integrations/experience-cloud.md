---
title: AEM as a Cloud Service と Adobe Experience Cloud の統合
description: AEM as a Cloud Service でサポートされているその他の Adobe Experience Cloud 製品との統合について説明します。
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
jira: KT-10718
thumbnail: KT-10718.png
last-substantial-update: 2022-11-17T00:00:00Z
mini-toc-levels: 1
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM as a Cloud Service" before-title="false"
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
duration: 135
source-git-commit: e5a1ab5fcc5314bddfbc4ad900127804bc019009
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 99%

---

# AEM as a Cloud Service と Adobe Experience Cloud の統合

AEM as a Cloud Service でサポートされているその他の Adobe Experience Cloud 製品との統合について説明します。
統合の設定方法と使用方法に関するドキュメントについては、Experience Cloud 製品をクリックします。

|                                                                   | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |           |            | ✔ |
| Advertising |           |            |          |
| [Analytics](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |           |            |          |
| Campaign Classic |           |            |          |
| Campaign Standard |           |            |          |
| [Commerce](#adobe-commerce) | ✔ | ✔ |          |
| Customer Journey Analytics |           |            |          |
| [Experience Platform タグ](#adobe-experience-platform-tags) | ✔ |            | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |           | ✔ |          |
| [Learning Manager](#adobe-learning-manager) | ✔ |            |          |
| Marketo Engage |           |            |          |
| Real-time CDP |           |            |          |
| [Target](#adobe-target) | ✔ |            |          |
| [Workfront](#adobe-workfront) |           | ✔ |          |


## Adobe Acrobat Sign

Adobe Acrobat Sign（旧称 Acrobat Sign）では、法務、販売、給与、人事などの領域に関するドキュメントを処理するワークフローを改善して、AEM Forms のアダプティブフォームで電子サインのワークフローを使用できます。

### AEM Forms

+ [Adobe Acrobat Sign 統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html?lang=ja)
+ [AEM Forms と Adobe Acrobat Sign のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html?lang=ja)

## Adobe Analytics

Adobe Analytics と AEM as a Cloud Service との統合により、カスタマージャーニーのどこからでもコンテンツアクティビティを追跡し、データを分析できます。さらに、多用途なレポート、予測インテリジェンスなどを取得できます。

### AEM Sites

+ [Adobe Analytics 統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html?lang=ja)
+ [AEM Sites と Analytics のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=ja)
+ アドビクライアントデータレイヤー（ACDL）

   + [AEM WCM コアコンポーネントでの ACDL の拡張](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=ja)
   + [ACDL と AEM WCM コアコンポーネントの統合](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html?lang=ja)
   + [ACDL を使用したイベント駆動型データの処理](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html?lang=ja)
   + [アドビクライアントデータレイヤー（ACDL）のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)

### AEM Assets

+ [Assets Insights の概要](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html?lang=ja)
+ [Assets Insights の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html?lang=ja#configure-asset-insights)
+ [Assets Insights のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html?lang=ja)

### AEM Forms

+ [Adobe Analytics 統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html?lang=ja)

### AEM Sites

+ [Adobe Campaign Classic との統合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html?lang=ja#configure-user)
+ [Adobe Experience Manager ニュースレターの作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html?lang=ja)
+ [AEM メールコアコンポーネントのドキュメント](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

Adobe Commerce と AEM as a Cloud Service の統合により、ブランドはコマースエクスペリエンスを差別化し、加速するオンライン支出を獲得するために、より迅速に拡張し革新することができます。AEM with Commerce は、Experience Manager で提供される、没入感のあるパーソナライズされたオムニチャネルエクスペリエンスを任意の数のコマースソリューションと組み合わせて、差別化されたエクスペリエンスをショッピングジャーニーのあらゆる部分に提供し、価値創出までの時間を短縮すると共にコンバージョンの向上を促します。

### AEM Sites

+ [AEM Content and Commerce ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html?lang=ja)


## Adobe Experience Platform のタグ

Adobe Experience Platform のタグは AEM とシームレスに統合され、[分析](#adobe-analytics)、[ターゲティング](#adobe-target)、マーケティングおよび顧客体験をエンゲージメントするのに必要な広告タグを簡単にデプロイして管理する方法を提供します。

### AEM Sites

+ [Experience Platform タグのユーザーガイド](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)
+ [Experience Platform タグのチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)

### AEM Forms

+ [Experience Platform タグのユーザーガイド](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)
+ [Experience Platform タグのチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)

## Adobe Journey Optimizer

Adobe Journey Optimizer は、単一のアプリケーションから数百万人の顧客を対象に、オムニチャネルキャンペーンと 1 対 1 のスケジュールを設定するのに役立ちます。インテリジェントな意思決定とインサイトを活用して、ジャーニー全体を最適化します。

### AEM Assets

+ [AEM Assets Essentials と Adobe Journey Optimizer の統合](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html?lang=ja)

## Adobe Learning Manager

Adobe Learning Manager（旧称 Adobe Captivate Prime）は、顧客や従業員に対してパーソナライズされた学習を提供します。

### AEM Sites

+ [AEM Sites と Adobe Learning Manager の統合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html?lang=ja)

### AEM Sites

+ [コンテンツフラグメントのテキストの要約](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html?lang=ja#summarizing-text)

### AEM Assets

+ [画像のスマートタグ付け](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=ja)
+ [画像のカスタムスマートタグ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html?lang=ja)
+ [ビデオのスマートタグ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html?lang=ja)
+ [スマート切り抜き](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html?lang=ja)
+ [ビジュアル検索](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html?lang=ja)

### AEM Forms

+ [自動フォーム変換サービス](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html?lang=ja)


## Adobe Target

Adobe Target は AEM as a Cloud Service と統合し、AEM のコンテンツを活用して、すべてのエンドユーザーに最適化された web エクスペリエンスを実現します。

### AEM Sites

+ [Adobe Target 統合の設定](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/personalization/setup/integrate-adobe-target)

+[Personalizationのユースケース &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/personalization/overview#use-cases)

+ エクスペリエンスフラグメントを Target に

   + [エクスペリエンスフラグメントを Target に公開](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=ja)
   + [エクスペリエンスフラグメントを JSON として Target に公開](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=ja)

+ [Target での AEM Context Hub の使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html?lang=ja#creating-an-adobe-target-audience-using-the-audience-console)

## Adobe Workfront

Adobe Workfront と AEM as a Cloud Service との統合により、デジタルアセットの作成、共同作業、ライフサイクル管理のプロセスが合理化されます。

### AEM Assets

+ [Workfront 拡張コネクタの設定](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=ja)
+ [Workfront 拡張コネクタのビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html?lang=ja)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentials のユーザーガイド](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront と Assets Essentials のビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=ja)
