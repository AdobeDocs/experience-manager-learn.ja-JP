---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# Adobe AnalyticsとCustomer Journey Analyticsの統合

{{analytics-description}}

{{customer-journey-analytics-description}}

Adobe AnalyticsとCustomer Journey Analyticsの統合には、次のような主なメリットがあります。

+ **包括的なインサイト** を顧客の行動や好みに追加します。
+ **シームレスなクロスチャネルトラッキング** 全体的な見方を
+ **統合されたデータとレポート** を使用して正確な分析を行うことができます。
+ **パーソナライゼーションの強化** 顧客エンゲージメントの向上
+ **リアルタイムデータインサイト** 迅速な意思決定を可能にします。

## 一般的な統合

<table>
    <thead>
        <tr>
            <td>Experience Cloud</td>
            <td>統合の条件</td>
            <td>用途</td>
            <td>一般的なユースケース</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics とCustomer Journey Analytics</a></td>
            <td>Experience Platformソースコネクタ</td>
            <td>
                <ul>
                    <li>TODO:この統合を使用して、Analytics データをレポートスイートからExperience Platformに取り込みます。</li>
                    <li>顧客プロファイルに対するデータの可用性がデータ収集時から 2 ～ 30 分になり、データレイクへの可用性が最大 90 分になる場合。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>簡単な、ユーザーインターフェイスによって開始されたワークフロー。</li>
                    <li>Analytics prop および eVar を新しい XDM フィールドにコピーするためのユーザーインターフェイスのマッピング。</li>
                    <li>リアルタイムの顧客プロファイルとCustomer Journey Analyticsから価値を得る最速の方法。</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">TODO をリンク：Analytics とCustomer Journey Analytics</a></td>
            <td>Experience Platformエッジ</td>
            <td>
                <ul>
                    <li>長期的な戦略を導入する場合。 AEP Web SDK、AEP Mobile SDK、Edge Network Server API を使用して、デバイスからExperience Platformに直接データを送信します。</li>
                    <li>新規のお客様または既存のお客様で、同じおよび次のページのパーソナライゼーションの使用例をサポートするために、顧客プロファイルに対して Analytics データを利用できる必要がある場合。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>収集したデータを使用して、使用事例をサポートするための最大レベルの制御を提供します。</li>
                    <li>クライアント側のデータは、XDM フィールドに簡単にマッピングできます。</li>
                    <li>リアルタイム顧客プロファイルに対するデータの可用性を最も迅速に実現します。</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
