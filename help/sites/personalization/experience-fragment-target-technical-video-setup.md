---
title: AEM でのエクスペリエンスフラグメントと Adobe Target の統合のセットアップ
description: Adobe Experience Manager 6.4 は、AEM と Target の間のパーソナライズ機能ワークフローを新たに認識します。AEM 内で作成されたエクスペリエンスは、HTML オファーとして Adobe Target に直接配信できるようになりました。これによりマーケターは、様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。
feature: Experience Fragments
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
doc-type: Technical Video
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
duration: 238
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '216'
ht-degree: 100%

---

# エクスペリエンスフラグメントと Adobe Target の統合のセットアップ{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 は、AEM と Target の間のパーソナライズ機能ワークフローを新たに認識します。AEM 内で作成されたエクスペリエンスは、HTML オファーとして Adobe Target に直接配信できるようになりました。これによりマーケターは、様々なチャネルをまたいでコンテンツをシームレスにテストし、パーソナライズできます。

>[!VIDEO](https://video.tv.adobe.com/v/328891?quality=12&learn=on&captions=jpn)

>[!NOTE]
>
>at.js クライアントライブラリの使用をお勧めします。ベストプラクティスは、Adobe Experience Platform のタグなどのタグ管理ソリューションやサードパーティのタグ管理ソリューションを使用して、サイトページにターゲットライブラリを追加することです。

* エクスペリエンスフラグメントフォルダーに適用される Target のクラウドサービス設定は、親フォルダーの直下に作成されたすべてのエクスペリエンスフラグメントに継承されます。子フォルダーは親クラウドサービス設定を継承しません。
* Target クライアントコードは、Adobe Experience Cloud／Launch Target／「設定」タブ／実装／at.js 設定を編集から取得できます。
* Target API のユーザー名とパスワードを取得するには、エクスペリエンスフラグメントと Target の統合機能を有効にすることを求めるリクエストを含んだチケットをクライアントケアに送信します。

## その他のリソース {#additional-resources}

* [エクスペリエンスフラグメントドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [エクスペリエンスフラグメントの使用](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
