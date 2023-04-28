---
title: AEM でのエクスペリエンスフラグメントと Adobe Target の統合のセットアップ
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager 6.4 は、AEM と Target の間のパーソナライズ機能ワークフローを新たに認識します。AEM 内で作成されたエクスペリエンスは、HTML オファーとして Adobe Target に直接配信できるようになりました。これによりマーケターは、様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 100%

---

# エクスペリエンスフラグメントと Adobe Target の統合のセットアップ{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 は、AEM と Target の間のパーソナライズ機能ワークフローを新たに認識します。AEM 内で作成されたエクスペリエンスは、HTML オファーとして Adobe Target に直接配信できるようになりました。これによりマーケターは、様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/22380?quality=12&learn=on)

>[!NOTE]
>
>at.js クライアントライブラリの使用をお勧めします。また、ベストプラクティスは、Experience Platform Launch、Adobe DTM、サードパーティのタグ管理ソリューションなどのタグ管理ソリューションを使用して、サイトページにターゲットライブラリを追加することです。

* エクスペリエンスフラグメントフォルダーに適用される Target のクラウドサービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントに継承されます。子フォルダーは親クラウドサービス設定を継承しません。
* Target クライアントコードは、Adobe Experience Cloud／Launch Target／「設定」タブ／実装／at.js 設定を編集から取得できます。
* Target API のユーザー名とパスワードを取得するには、エクスペリエンスフラグメントと Target の統合機能を有効にすることを求めるリクエストを含んだチケットをクライアントケアに送信します。

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
