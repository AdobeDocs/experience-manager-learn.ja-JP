---
title: DoS/DDoS 攻撃防止について
description: AEM に対する DoS 攻撃と DDoS 攻撃を防止および軽減する方法について説明します。
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: f84a6cc54838e5bf87cfa173ef17df4ec745ebcb
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# AEM での DoS/DDoS 攻撃防止について

AEM 環境に対する DoS 攻撃と DDoS 攻撃を防止および軽減するために使用できるオプションについて説明します。防止メカニズムに入る前に、[DoS](https://developer.mozilla.org/ja-JP/docs/Glossary/DOS_attack) と [DDoS](https://developer.mozilla.org/ja-JP/docs/Glossary/Distributed_Denial_of_Service) について簡単に概要を説明します。

- DoS（サービス拒否）攻撃と DDoS（分散型のサービス拒否）攻撃はどちらも、対象となるサーバー、サービス、またはネットワークの通常の機能を妨害し、対象のユーザーによるアクセスを阻害しようとする悪意のある試みです。
- DoS 攻撃は通常、1 つのソースから発生しますが、DDoS 攻撃は複数のソースから発生します。
- DDoS 攻撃は、複数の攻撃デバイスのリソースが組み合わせるので、DoS 攻撃と比較して大規模になりがちです。
- これらの攻撃は、ターゲットに過剰なトラフィックを大量に送り込み、ネットワークプロトコルの脆弱性を悪用することで実行されます。

次の表では、DoS 攻撃と DDoS 攻撃を防止および軽減する方法について説明します。

<table>
    <tbody>
        <tr>
            <td><strong>防止メカニズム</strong></td>
            <td><strong>説明</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5（AMS）</strong></td>
            <td><strong>AEM 6.5（オンプレミス）</strong></td>
        </tr>
        <tr>
            <td>Web アプリケーションファイアウォール（WAF）</td>
            <td>Web アプリケーションを様々なタイプの攻撃から保護するために設計されたセキュリティソリューション。</td>
            <td>
            <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS 保護ライセンス</a></td>
            <td>AMS 契約による <a href="ttps://docs.aws.amazon.com/waf/" target="_blank">AWS</a> または <a href="https://azure.microsoft.com/ja-jp/products/web-application-firewall" target="_blank">Azure</a> WAF。</td>
            <td>推奨される WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity（別名「mod_security」Apache モジュール）は、web アプリケーションに対する様々な攻撃から保護する、オープンソースのクロスプラットフォームソリューションです。<br/> AEM as a Cloud Service では、AEM オーサーサービスの前に Apache Web サーバーと AEM Dispatcher がないので、AEM パブリッシュサービスにのみ適用されます。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity を有効にする </a></td>
        </tr>
        <tr>
            <td>トラフィックフィルタールール</td>
            <td>トラフィックフィルタールールを使用して、CDN レイヤーでリクエストをブロックまたは許可できます。</td>
            <td><a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">トラフィックフィルタールールの例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> または <a href="https://learn.microsoft.com/ja-jp/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> のルール制限機能。</td>
            <td>推奨されるソリューション</td>
        </tr>
    </tbody>
</table>

## インシデント後の分析と継続的な改善

DoS/DDoS 攻撃を特定して防止するための万能の標準フローはなく、組織のセキュリティプロセスによって異なります。**インシデント後の分析と継続的な改善**&#x200B;は、プロセスにおける重要なステップです。考慮すべきベストプラクティスは次のとおりです。

- ログ、ネットワークトラフィック、システム設定の確認など、インシデント後の分析を行って、DoS/DDoS 攻撃の根本原因を特定します。
- インシデント後の分析の結果に基づいて、防止メカニズムを改善します。

