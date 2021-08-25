---
title: Adobe Target内でのAEMエクスペリエンスフラグメントオファーの使用
description: Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
feature: Experience Fragments
version: 6.4, 6.5
topic: Personalization
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---


# Adobe Target内でのエクスペリエンスフラグメントオファーの使用{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Managerは、AEMとTargetの間のパーソナライゼーションワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>`at.js`クライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトページにターゲットライブラリを追加することです


* Adobe Experience Managerの使いやすく強力なコンテンツ作成メカニズム、Adobe Targetの人工知能(AI)と機械学習に加えて、コンテンツ作成者は、すべてのチャネルのコンテンツを一元的に作成および管理できます。 エクスペリエンスフラグメントをAdobe TargetにHTMLオファーとして書き出す機能により、マーケターは、これらのオファーを使用して、よりパーソナライズされたエクスペリエンスを柔軟に作成し、作成する各エクスペリエンスをテストおよび拡大できます。
* HTMLオファーとエクスペリエンスフラグメントオファーの主な違いは、後での編集はAEMでのみおこなえ、その後Adobe Targetと同期できる点です
* エクスペリエンスフラグメントフォルダーに適用されたTarget Cloudサービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントを継承します。 子フォルダーは親クラウドサービスの構成を継承しません。
* パーソナライズされたオファーを作成するために、AEM内に保存されたコンテンツを簡単に活用できるようになりました。
* 自動配分、自動ターゲット、Automated PersonalizationなどのSenseiを利用したアクティビティを含む、Targetアクティビティのタイプを作成できます

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
