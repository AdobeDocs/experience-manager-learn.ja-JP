---
title: Analytics とReal-time Customer Data PlatformのExperience Platformソースコネクタとの統合チュートリアル
description: Adobe AnalyticsとReal-time Customer Data Platformを統合する方法を説明します。
solution: Real-Time Customer Data Platform, Analytics
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="統合" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---


# Adobe AnalyticsとReal-time Customer Data PlatformのExperience Platformソースコネクタとの統合

<ol>
    <li><a href="https://experienceleague.adobe.com/?lang=en#dashboard/learning" _target="_blank" rel="noopener noreferrer">スキーマを作成</a> 取り込むデータの</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html" _target="_blank" rel="noopener noreferrer">データセットの作成</a> 取り込むデータの</a></li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/identities/label-ingest-and-verify-identity-data.html?lang=en" _target="_blank" rel="noopener noreferrer">スキーマでの正しい ID と ID 名前空間の設定</a> 取り込んだデータを統合プロファイルに確実に結び付けることができるようにするため。</li> 
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html" _target="_blank" rel="noopener noreferrer">プロファイルのスキーマとデータセットの有効化</a>.</li>
    <li>次のいずれかの方法を使用して、Experience Platformにデータを取り込みます。</li>
        <ul>
            <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">Adobe Analyticsソースコネクタ</a></li>
            <li>Experience PlatformWeb SDK:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html" _target="_blank" rel="noopener noreferrer">チュートリアル</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/web-sdk/overview.html" _target="_blank" rel="noopener noreferrer">チェックリスト</a></li>
                </ul>
            <li>Experience Platformモバイル SDK:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/data-collection/mobile-sdk/create-mobile-properties.html" _target="_blank" rel="noopener noreferrer">チュートリアル</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/mobile-sdk/overview.html" _target="_blank" rel="noopener noreferrer">チェックリスト</a></li>
                </ul></li>
            <li>Edge Network Server API:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/interacting-other-adobe-solutions/interacting-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">チュートリアル</a></li>
                </ul>
       </ul>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html" _target="_blank" rel="noopener noreferrer">「 」でセグメントをExperience Platformします。</a> セグメントがバッチ（データコネクタ）とストリーミング（Edge ネットワーク）のどちらで評価されるかは、システムによって自動的に判断されます。</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/destinations/create-destinations-and-activate-data.html" _target="_blank" rel="noopener noreferrer">目的の宛先に対して、プロファイル属性とオーディエンスメンバーシップを共有するための宛先を設定します。</a></li>   
</ol>

>[!NOTE]
>
>Adobe Analyticsソースコネクタの標準ワークフロー手順では、Analytics からデータを「現状のまま」取り込むために使用するスキーマとデータセットを作成します。 したがって、最初の 2 つの手順はシステムによって処理されます。 マッピングワークフローでは、カスタム属性を作成する必要があります。したがって、手順の順序に完全に従います。