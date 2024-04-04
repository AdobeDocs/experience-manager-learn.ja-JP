---
title: DoS/DoS 防止について
description: AEMに対する DoS および DoS 攻撃を防ぎ、軽減する方法を説明します。
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
ht-degree: 1%

---

# AEMでの DoS/DoS 防止について

AEM環境での DoS および DoS 攻撃を防ぎ、軽減するために使用できるオプションについて説明します。 予防メカニズムに取り組む前に、 [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) および [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- DoS(DoS(DoS) および DDoS(Distributed DoS) 攻撃は、ターゲットサーバー、サービス、またはネットワークの通常の機能を妨害し、意図したユーザーからアクセスできないようにする悪意のある試みです。
- DoS 攻撃は通常 1 つのソースから発生し、DoS 攻撃は複数のソースから発生します。
- DDoS 攻撃は、多くの場合、複数の攻撃デバイスのリソースを組み合わせることで、DoS 攻撃に比べて大規模です。
- これらの攻撃は、過剰なトラフィックでターゲットを満たし、ネットワークプロトコルの脆弱性を悪用することによって実行されます。

次の表に、DoS および DoS 攻撃を防ぎ、軽減する方法を示します。

<table>
    <tbody>
        <tr>
            <td><strong>防止機構</strong></td>
            <td><strong>説明</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5(AMS)</strong></td>
            <td><strong>AEM 6.5（オンプレミス）</strong></td>
        </tr>
        <tr>
            <td>Web アプリケーションファイアウォール (WAF)</td>
            <td>様々な種類の攻撃から Web アプリケーションを保護するように設計されたセキュリティソリューション。</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS 保護ライセンス</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> または <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> AMS 契約を介した WAF。</td>
            <td>お好みの WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity（別名「mod_security」Apache モジュール）は、Web アプリケーションに対する様々な攻撃から保護する、オープンソースのクロスプラットフォームソリューションです。<br/> AEM as a Cloud Serviceでは、AEMオーサーサービスの前に Apache Web サーバーとAEM Dispatcher がないので、これはAEMパブリッシュサービスにのみ適用されます。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity を有効にする </a></td>
        </tr>
        <tr>
            <td>トラフィックフィルタールール</td>
            <td>トラフィックフィルタールールを使用して、CDN レイヤーでリクエストをブロックまたは許可できます。</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">トラフィックフィルタールールの例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> または <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> ルール制限機能を使用します。</td>
            <td>お客様が希望するソリューション</td>
        </tr>
    </tbody>
</table>

## インシデント後の分析と継続的な改善

DoS/DoS 攻撃を識別および防止するための 1 つのサイズに合った標準フローはありませんが、これは組織のセキュリティプロセスに依存します。 The **事後分析と継続的改善** はプロセスの重要なステップです。 以下に、考慮すべきベストプラクティスを示します。

- ログ、ネットワークトラフィック、システム設定の確認など、インシデント発生後の分析を実行して、DoS/DoS 攻撃の根本原因を特定します。
- 事後インシデント分析の結果に基づいて予防メカニズムを改善する。

