---
title: Adobe Target内でのAEMエクスペリエンスフラグメントオファーの使用
seo-title: Adobe Target内でのAEMエクスペリエンスフラグメントオファーの使用
description: Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
seo-description: Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
sub-product: content-services
feature: エクスペリエンスフラグメント
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: パーソナライズ機能
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 12%

---


# Adobe Target{#using-experience-fragment-offers-within-adobe-target}内でのエクスペリエンスフラグメントオファーの使用

Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>at.jsクライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトページにターゲットライブラリを追加することです

>[!NOTE]
>
>Adobe Target内のAEMエクスペリエンスフラグメントオファーは、AEM 6.3ユーザー向けの機能パックとしても利用できます。 機能パックと依存関係については、以下の節を参照してください。


* Adobe Experience Managerの使いやすく強力なコンテンツ作成メカニズム、Adobe Targetの人工知能(AI)と機械学習に加えて、コンテンツ作成者は、すべてのチャネルのコンテンツを一元的に作成および管理できます。 エクスペリエンスフラグメントをAdobe TargetにHTMLオファーとして書き出す機能により、マーケターは、これらのオファーを使用して、よりパーソナライズされたエクスペリエンスを柔軟に作成し、作成する各エクスペリエンスをテストおよび拡大できます。
* HTMLオファーとエクスペリエンスフラグメントオファーの主な違いは、後での編集はAEMでのみおこなえ、その後Adobe Targetと同期できる点です
* エクスペリエンスフラグメントフォルダーに適用されたTarget Cloudサービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントを継承します。 子フォルダーは親クラウドサービスの構成を継承しません。
* パーソナライズされたオファーを作成するために、AEM内に保存されたコンテンツを簡単に活用できるようになりました。
* 自動配分、自動ターゲット、Automated PersonalizationなどのSenseiを利用したアクティビティを含む、Targetアクティビティのタイプを作成できます

## AEM 6.3機能パックと依存関係{#aem-feature-packs-and-dependencies}

| AEM 6.3 機能パック | 依存関係 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントのドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
