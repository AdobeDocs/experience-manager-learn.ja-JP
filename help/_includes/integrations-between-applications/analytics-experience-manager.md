---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 5%

---


# 分析とExperience Managerの統合

{{analytics-description}}

{{experience-manager-description}}

Adobe AnalyticsとAdobe Experience Managerの統合には、次のような利点があります。

+ **正確なセグメント化**:Adobe AnalyticsとAudience Managerを統合して、キャンペーンのパーソナライズされたオーディエンスセグメントを作成します。
+ **包括的な顧客** プロファイル：データソースを統合して、インタラクションと行動を統一的に理解します。
+ **最適化された広告ターゲティング**:Adobe AnalyticsとAudience Managerのデータ駆動型ターゲティングにより、広告の効果を高めます。
+ **情報に基づく意思決定**:より良い選択のために、結合されたAdobe AnalyticsとAudience Managerのデータから詳細なインサイトを得る。
+ **パーソナライズされたエクスペリエンス**:両方のプラットフォームの機能を活用し、タッチポイントをまたいでパーソナライズされたコンテンツとオファー。

## 一般的な統合

<table>
    <thead>
        <tr>
            <th>Experience Cloud</th>
            <th>統合の条件</th>
            <th>用途</th>
            <th>一般的なユースケース</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/analytics-using-web-sdk.html" target="_blank" rel="noreferrer">AEM Sitesと Analytics</a></td>
            <td>Experience PlatformWeb SDK タグ拡張機能または alloy.js</td>
            <td>
                <ul>
                    <li>Adobe AnalyticsでAEM Web 分析データに関するレポートを作成し、将来他のExperience Cloudアプリケーションと統合するように配置します。</li>
                </ul>
            </td>
            <td>
                <ul>
                  <li>Web サイトトラフィックの追跡。</li>
                  <li>マーケティングキャンペーンの監視。</li>
                  <li>Web サイトのパフォーマンスを最適化する。</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=ja" target="_blank" rel="noreferrer">AEM Sitesと Analytics</a></td>
            <td>Adobe Analyticsタグ拡張機能またはAppMeasurement.js</td>
            <td>
                <ul>
                    <li>Adobe Analyticsで Web 分析データのみが必要な場合。</li>
                    <li>追跡可能な Web サイト要素にAEMコアコンポーネントを使用する場合。</li>
                    <li>最小限の設定と実装が必要な場合。</li>
                </ul>
            </td>
            <td>
                <ul>
                  <li>Web サイトトラフィックの追跡。</li>
                  <li>マーケティングキャンペーンの監視。</li>
                  <li>Web サイトのパフォーマンスを最適化する。</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/forms-and-analytics/introduction.html" target="_blank" rel="noreferrer">Analytics とAEM FormsをCloud Service</a></td>
            <td>Experience PlatformWeb SDK タグ拡張機能または alloy.js</td>
            <td>
              <ul>
                <li>Adobe Analyticsでデジタルフォームの分析データをレポートする場合は、将来他のExperience Cloudアプリケーションと統合するように自分で配置します。</li>
              </ul>
            </td>
            <td>
                <ul>
                  <li>フォームの送信を追跡します。</li>
                  <li>フォームフィールドのエラーを監視する。</li>
                  <li>送信済みフォームフィールドの値に関するレポート。</li>
                </ul>
            </td>
        </tr>
    </tbody>          
</table>
