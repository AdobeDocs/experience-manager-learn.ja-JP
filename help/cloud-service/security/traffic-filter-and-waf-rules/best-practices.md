---
title: WAF ルールを含むトラフィックフィルタールールのベストプラクティス
description: セキュリティを強化しリスクを軽減するために、AEM as a Cloud ServiceのWAF ルールを含むトラフィックフィルタールールを設定するための推奨されるベストプラクティスについて説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# WAF ルールを含むトラフィックフィルタールールのベストプラクティス

セキュリティを強化しリスクを軽減するために、AEM as a Cloud ServiceのWAF ルールを含むトラフィックフィルタールールを設定するための推奨されるベストプラクティスについて説明します。

>[!IMPORTANT]
>
>この記事で説明されているベストプラクティスは完全なものではなく、独自のセキュリティポリシーおよび手順の代わりとなることを目的としていません。

## 一般的なベストプラクティス

- Adobeが提供する標準トラフィックフィルターとWAF ルールの [ 推奨セット ](./overview.md#adobe-recommended-rules) から始めて、アプリケーション固有のニーズと脅威の状況に基づいて微調整します。
- セキュリティチームと協力して、組織のセキュリティ体制およびコンプライアンス要件に合致するルールを決定します。
- 新規ルールまたは更新されたルールは、ステージング環境および実稼動環境に昇格する前に、常に開発環境でテストしてください。
- ルールの宣言と検証を行う場合は、まず `action` タイプ `log` から開始して、正当なトラフィックをブロックせずに動作を確認します。
- 十分なトラフィックデータを分析し、有効なリクエストが影響を受けていないことを確認した後にのみ、`log` から `block` に移動します。
- 意図しない副作用を特定するために、QA、パフォーマンス、およびセキュリティテストチームを含め、ルールを段階的に導入します。
- [ ダッシュボードツール ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) を使用して、ルールの有効性を定期的にレビューおよび分析します。 レビューの頻度（毎日、毎週、毎月）は、サイトのトラフィック量とリスクプロファイルに合わせる必要があります。
- 新しい脅威情報、トラフィック行動、監査結果に基づいてルールを継続的に調整します。

## トラフィックフィルタールールのベストプラクティス

- Adobe[ 推奨される標準トラフィックフィルタールール ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) をベースラインとして使用します。これには、エッジ、接触チャネル保護、OFAC ベースの制限のルールが含まれます。
- アラートおよびログを定期的に確認して、不正使用や設定ミスのパターンを特定します。
- アプリケーションのトラフィックパターンとユーザーの行動に基づいて、レート制限のしきい値を調整します。

  しきい値の選択方法のガイダンスについては、次の表を参照してください。

  | バリエーション | 値 |
  | :--------- | :------- |
  | 接触チャネル | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大接触チャネルリクエストの最大値を取得し、倍数で増やします。 |
  | Edge | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大 Edge リクエストの最大値を取得し、倍数で増やします。 |

  詳しくは、[ しきい値の選択 ](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) の節も参照してください。

- `block` のアクションが正当なトラフィックに影響を与えないことを確認した後にのみ、`log` のアクションに移動します。

## WAF ルールのベストプラクティス

- Adobe[ 推奨されるWAFのルール ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) から始めます。これには、既知の不正な IP をブロックするルール、DDoS 攻撃を検出するルール、ボットの不正使用を軽減するルールが含まれます。
- `ATTACK` WAFは、潜在的な脅威に対して警告する必要があります。 `block` に移行する前に、誤検出がないことを確認します。
- 推奨されるWAFのルールが具体的な脅威に対応していない場合は、アプリケーション固有の要件に基づいてカスタムルールを作成することを検討してください。 [WAF フラグ ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) の完全なリストについては、ドキュメントを参照してください。

## ルールの実装

AEM as a Cloud ServiceにトラフィックフィルタールールとWAF ルールを実装する方法について説明します。

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="標準のトラフィックフィルタールールを使用したAEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="標準のトラフィックフィルタールールを使用したAEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="標準のトラフィックフィルタールールを使用したAEM web サイトの保護"> 標準のトラフィックフィルタールールを使用したAEM web サイトの保護 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud ServiceでAdobeが推奨する標準トラフィックフィルタールールを使用して、DoS、DDoS、ボットの不正使用からAEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> ルールの適用 </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="WAF ルールを使用したAEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="WAF ルールを使用したAEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="WAF ルールを使用したAEM web サイトの保護">WAF ルールを使用したAEM web サイトの保護 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud ServiceでAdobeが推奨する Web Application Firewall （WAF）ルールを使用して、DoS、DDoS、ボットの不正使用などの高度な脅威からAEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAFのアクティブ化 </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## その他のリソース

- [WAFルールを含むトラフィックフィルタールール ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [AEMでの DoS/DDoS 防止について ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [ トラフィックフィルタールールを使用した DoS および DDoS 攻撃のブロック ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

