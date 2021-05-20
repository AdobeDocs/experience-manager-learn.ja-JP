---
title: AEMでのエクスペリエンスフラグメントとAdobe Target統合のセットアップ
seo-title: AEMでのエクスペリエンスフラグメントとAdobe Target統合のセットアップ
description: Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
seo-description: Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
sub-product: content-services
feature: エクスペリエンスフラグメント
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: パーソナライズ機能
role: Administrator, Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 2%

---


# エクスペリエンスフラグメントとAdobe Target統合のセットアップ{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4では、AEMとTargetの間のパーソナライゼーションワークフローが再設計されました。 AEM内で作成されたエクスペリエンスを、HTMLオファーとしてAdobe Targetに直接配信できるようになりました。 これにより、マーケターは様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>at.jsクライアントライブラリの使用をお勧めします。ベストプラクティスは、Launch by Adobe、AdobeDTM、またはサードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトページにターゲットライブラリを追加することです

* エクスペリエンスフラグメントフォルダーに適用されたTarget Cloudサービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントを継承します。 子フォルダーは親クラウドサービスの構成を継承しません。
* Targetクライアントコードは、 Adobe Experience Cloud / LaunchのTarget /「セットアップ」タブ/実装/ at.js設定を編集から取得できます。
* Target APIのユーザー名とパスワードは、エクスペリエンスフラグメントのTarget統合機能を有効にするリクエストを含むチケットをClientCareに送信することで取得できます。

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントのドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
