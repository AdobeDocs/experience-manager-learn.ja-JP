---
title: Adobe TargetでのAEMエクスペリエンスフラグメントオファーの使用
seo-title: Adobe TargetでのAEMエクスペリエンスフラグメントオファーの使用
description: Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。
seo-description: Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。
sub-product: content-services
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# Adobe Target内でのエクスペリエンスフラグメントオファーの使用{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>at.jsクライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのtag managementソリューションなどのtag managementソリューションを使用して、サイトページにターゲットライブラリを追加することです

>[!NOTE]
>
>ADOBE TARGETのAEM Experience Fragmentオファーは、AEM 6.3ユーザー向けの機能パックとしても利用できます。 フィーチャパックと依存関係については、次のセクションを参照してください。


* Adobe Experience Managerの使いやすくパワフルなコンテンツ作成メカニズム、Adobe TargetのArtifical Intelligence(AI)とMachine Learningは、コンテンツ作成者が一元的にすべてのチャネルのコンテンツを作成および管理できるようにします。 エクスペリエンスフラグメントをHTMLオファーとしてAdobe Targetに書き出す機能により、マーケティング担当者は、これらのオファーを使用してパーソナライズされたエクスペリエンスをより柔軟に作成でき、作成した各エクスペリエンスをテストおよび拡大できます。
* HTMLオファーとエクスペリエンスフラグメントオファーの主な違いは、後で行うための編集はAEMでのみ行え、Adobe Targetと同期できることです
* エクスペリエンスフラグメントフォルダーに適用されるターゲットクラウドサービス設定は、親フォルダーの直下に作成されるすべてのエクスペリエンスフラグメントに継承されます。 子フォルダーは、親のクラウドサービスの構成を継承しません。
* パーソナライズされたオファーを作成するために、AEM内に保存されたコンテンツを簡単に活用できるようになりました。
* 自動配分、自動ターゲット、Automated Personalizationなど先生を使用したアクティビティを含む、ターゲットアクティビティのタイプを作成できます

## AEM 6.3機能パックと依存関係 {#aem-feature-packs-and-dependencies}

| AEM 6.3 機能パック | 依存関係 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
