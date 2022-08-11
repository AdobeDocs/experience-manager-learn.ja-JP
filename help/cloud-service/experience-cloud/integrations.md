---
title: Adobe Experience CloudとのAEMas a Cloud Service統合
description: AEMas a Cloud Serviceの他のAdobe Experience Cloud製品との統合に関する詳細
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.jpeg
mini-toc-levels: 1
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: 663075723da207242309c08feed42657b9e5188b
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 18%

---

# Adobe Experience CloudとのAEMas a Cloud Service統合

AEMas a Cloud Serviceの他のAdobe Experience Cloud製品との統合に関する詳細
統合の設定方法と使用方法に関するドキュメントについては、Experience Cloud製品をクリックしてください。

|  | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |  |  | ✔ |
| 広告 |  |  |  |
| [Analytics](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |  |  |  |
| [Campaign Classic](#adobe-campaign-classic) | ✔ |  |  |
| Campaign Standard |  |  |  |
| [コマース](#adobe-commerce) | ✔ | ✔ |  |
| Customer Journey Analytics |  |  |  |
| [Experience Platformタグ](#adobe-experience-platform-tags) | ✔ |  | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |  | ✔ |  |
| [ラーニングマネージャー](#adobe-learning-manager) | ✔ |  |  |
| Marketo Engage |  |  |  |
| リアルタイム CDP |  |  |  |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [ターゲット](#adobe-target) | ✔ |  |  |
| [Workfront](#adobe-workfront) |  | ✔ |  |


## Adobe Acrobat Sign

Adobe Acrobat Sign( 旧称Adobe Sign) では、AEM Forms のアダプティブフォームで電子署名ワークフローを使用して、法務、販売、給与、人事などの領域に関するドキュメントを処理するワークフローを改善できます。

### AEM Forms

+ [Adobe Acrobat Sign統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM FormsとAdobe Acrobat Signのチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

Adobe AnalyticsとAEM as a Cloud Serviceの統合により、コンテンツのアクティビティを追跡し、カスタマージャーニーの任意の場所からデータを分析できます。 さらに、多用途なレポート、予測インテリジェンスなどを取得できます。

### AEM Sites

+ [Adobe Analytics統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sitesと Analytics のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=ja)
+ Adobeクライアントデータレイヤー (ACDL)

   + [AEM WCM コアコンポーネントでの ACDL の拡張](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [ACDL とAEM WCM コアコンポーネントの統合](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html?lang=ja)
   + [ACDL を使用したイベント駆動型データの処理](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Adobeクライアントデータレイヤー (ACDL) のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=ja)

### AEM Assets

+ [アセットインサイトの概要](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [アセットインサイトの設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [Assets Insights チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html?lang=ja)

### AEM Forms

+ [Adobe Analytics統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

## Adobe Campaign Classic

Adobe Campaign ClassicとAEM as a Cloud Serviceの統合により、Adobe Campaign Classicを使用して E メールをパーソナライズおよび配信しながら、Adobe Experience Managerで E メール配信のコンテンツとフォームを直接管理できます。

### AEM Sites

+ [Adobe Campaign Classic との統合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html#configure-user)
+ [Adobe Experience Managerニュースレターの作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html)
+ [AEM E メールコアコンポーネントドキュメント](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

Adobe CommerceとAEM as a Cloud Serviceの統合により、企業はより迅速に拡張および革新を行い、コマースエクスペリエンスを区別し、迅速なオンライン支出を獲得することができます。 AEM with Commerce は、あらゆる数のコマースソリューションとExperience Managerして、没入感のあるオムニチャネルとパーソナライズされたエクスペリエンスを組み合わせ、ショッピングジャーニーのすべての部分に差別化されたエクスペリエンスを提供し、価値創出と高いコンバージョンを促進します。

### AEM Sites

+ [AEM Content and Commerce ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platformタグ

Adobe Experience Platformタグ ( 旧Adobeの Launch、DTM) はAEMとシームレスに統合され、シンプルなデプロイおよび管理方法を提供します [分析](#adobe-analytics), [ターゲット](#adobe-target)顧客体験をエンゲージメントするのに必要な、マーケティングおよび広告のタグ。

### AEM Sites

+ [Experience Platformタグユーザーガイド](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platformタグのチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)

### AEM Forms

+ [Experience Platformタグユーザーガイド](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platformタグのチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

## Adobe Journey Optimizer

Adobe Journey Optimizerは、単一のアプリケーションから数百万人の顧客を対象に、オムニチャネルキャンペーンと 1 対 1 のスケジュールを設定し、インテリジェントな判定とインサイトを活用してジャーニー全体を最適化します。

### AEM Assets

+ [AEM Assets Essentials とAdobe Journey Optimizerの統合](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html?lang=ja)

## Adobeラーニングマネージャー

Adobeラーニングマネージャー ( 旧称Adobe Captivate Prime) は、顧客や従業員に対してパーソナライズされたラーニングを提供します。

### AEM Sites

+ [AEM SitesとAdobeラーニングマネージャーの統合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html)

## Adobe Sensei

Adobe Senseiは、スマートタグ、スマート切り抜き、ビジュアル検索などを使用してコンテンツ管理のプロセスを変換する AI および機械学習テクノロジーを提供します。

### AEM Sites

+ [コンテンツフラグメントのテキストの要約](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [画像のスマートタグ付け](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=ja)
+ [画像のカスタムスマートタグ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [ビデオのスマートタグ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [スマート切り抜き](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [ビジュアル検索](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [自動フォーム変換サービス](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe TargetはAEM as a Cloud Serviceと統合され、AEMのコンテンツを活用して、あらゆるエンドユーザーに最適化された web エクスペリエンスを提供します。

### AEM Sites

+ [Adobe Target統合の設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ エクスペリエンスフラグメントをターゲットに

   + [エクスペリエンスフラグメントを Target に公開する](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [エクスペリエンスフラグメントを JSON として Target に公開する](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [Target でのAEM Context Hub の使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sitesと Target のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)

## Adobe Workfront

Adobe WorkfrontとAEM sCloud Serviceとの統合により、デジタルアセットの作成、コラボレーション、ライフサイクル管理のプロセスが合理化されます。

### AEM Assets

+ [Workfront拡張コネクタの設定](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=ja)
+ [Workfront enhanced connector ビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentialsユーザーガイド](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe WorkfrontとAssets Essentialsのビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
