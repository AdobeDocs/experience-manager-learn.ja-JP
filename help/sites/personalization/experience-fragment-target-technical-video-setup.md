---
title: AEMでのエクスペリエンスフラグメントとAdobe Target統合のセットアップ
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager 6.4 では、AEMと Target の間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
seo-description: Adobe Experience Manager 6.4 reimagines the personalization workflow between AEM and Target. Experiences created within AEM can now be delivered directly to Adobe Target as HTML Offers. It allows Marketers to seamlessly test and personalize content across different channels.
feature: Experience Fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 2%

---

# エクスペリエンスフラグメントとAdobe Target統合の設定{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 では、AEMと Target の間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>at.js クライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトのページに target ライブラリを追加することです

* エクスペリエンスフラグメントフォルダーに適用された Target Cloud サービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントを継承します。 子フォルダーは親クラウドサービス設定を継承しません。
* Target クライアントコードは、Adobe Experience Cloud/Launch Target/「セットアップ」タブ/実装/ at.js 設定を編集から取得できます。
* Target API のユーザー名とパスワードは、エクスペリエンスフラグメントの Target 統合機能を有効にするリクエストを含むチケットを ClientCare に送信することで取得できます。

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
