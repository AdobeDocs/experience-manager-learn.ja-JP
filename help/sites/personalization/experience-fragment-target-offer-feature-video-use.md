---
title: Adobe Target内でのAEM Experience Fragment オファーの使用
description: Adobe Experience Manager 6.4 では、AEMと Target の間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
feature: Experience Fragments
version: 6.4, 6.5
topic: Personalization
role: User
level: Beginner
exl-id: 9ee826cf-389f-4570-bfe1-0d43d3fed3e1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---

# Adobe Target内でのエクスペリエンスフラグメントオファーの使用{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Managerは、AEMと Target の間のパーソナライゼーションワークフローを再設計します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22383?quality=12&learn=on)

>[!NOTE]
>
>使用を推奨 `at.js` クライアントライブラリとベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトのページに target ライブラリを追加することです


* Adobe Experience Managerの使いやすく強力なコンテンツ作成メカニズムと、Adobe Targetの人工知能 (AI) および機械学習を使用すると、コンテンツ作成者はすべてのチャネルのコンテンツを一元的に作成および管理できます。 エクスペリエンスフラグメントをHTMLオファーとしてAdobe Targetに書き出す機能により、マーケターは、これらのオファーを使用して、よりパーソナライズされたエクスペリエンスを柔軟に作成し、作成する各エクスペリエンスのテストと拡張をおこなうことができます。
* HTMLオファーとエクスペリエンスフラグメントオファーの主な違いは、後での編集はAEMでのみおこなえ、その後Adobe Targetと同期できることです
* エクスペリエンスフラグメントフォルダーに適用された Target Cloud サービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントを継承します。 子フォルダーは親クラウドサービス設定を継承しません。
* パーソナライズされたオファーを作成するために、AEM内に保存されたコンテンツを簡単に活用できるようになりました。
* 自動配分、自動ターゲット、Automated Personalizationなど、Senseiを利用したアクティビティを含む、Target アクティビティのタイプを作成できます

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
