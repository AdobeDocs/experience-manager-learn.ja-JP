---
title: AEMでのエクスペリエンスフラグメントとAdobe Target統合の設定
seo-title: AEMでのエクスペリエンスフラグメントとAdobe Target統合の設定
description: Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。
seo-description: Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。
sub-product: content-services
feature: experience-fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# エクスペリエンスフラグメントとAdobe Target統合のセットアップ{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager6.4は、AEMとターゲットの間のパーソナライズワークフローを再考します。 AEM内で作成されたエクスペリエンスを、HTMLオファーとして直接Adobe Targetに配信できるようになりました。 これにより、マーケティング担当者は、様々なチャネルでコンテンツをシームレスにテストし、パーソナライズすることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>at.jsクライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのtag managementソリューションなどのtag managementソリューションを使用して、サイトページにターゲットライブラリを追加することです

* エクスペリエンスフラグメントフォルダーに適用されるターゲットクラウドサービス設定は、親フォルダーの直下に作成されるすべてのエクスペリエンスフラグメントに継承されます。 子フォルダーは、親のクラウドサービスの構成を継承しません。
* ターゲットクライアントコードは、Adobe Experience Cloud/起動ターゲット/「設定」タブで、実装/at.js設定を編集から取得できます。
* ターゲットAPIのユーザー名とパスワードは、エクスペリエンスフラグメントターゲット統合機能を有効にするリクエストを含むClientCareにチケットを送信することで取得できます。

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
