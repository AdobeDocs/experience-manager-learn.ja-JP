---
title: WAF ルールを含むトラフィックフィルタールールのベストプラクティス
description: セキュリティを強化し、リスクを軽減するために、AEM as a Cloud Service で WAF ルールを含むトラフィックフィルタールールを設定するための推奨されるベストプラクティスについて説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
source-git-commit: 71454ea9f1302d8d1c08c99e937afefeda2b1322
workflow-type: ht
source-wordcount: '638'
ht-degree: 100%

---

# WAF ルールを含むトラフィックフィルタールールのベストプラクティス

セキュリティを強化し、リスクを軽減するために、AEM as a Cloud Service で WAF ルールを含むトラフィックフィルタールールを設定するための推奨されるベストプラクティスについて説明します。

>[!IMPORTANT]
>
>この記事に記載されているベストプラクティスは完全なものではなく、独自のセキュリティポリシーや手順の代用になるものではありません。

## 一般的なベストプラクティス

- まず、アドビが提供する標準トラフィックフィルタールールと WAF ルールの[推奨されるセット](./overview.md#adobe-recommended-rules)から開始して、アプリケーション固有のニーズと脅威の状況に基づいて微調整します。
- セキュリティチームと共同作業して、組織のセキュリティ態勢とコンプライアンス要件に一致するルールを決定します。
- 新規ルールや更新済みのルールをステージ環境と実稼動環境に昇格させる前に、常に開発環境でテストします。
- ルールを宣言および検証する際は、`action` タイプ `log` から開始して、正当なトラフィックをブロックせずに動作を確認します。
- 十分なトラフィックデータを分析し、有効なリクエストが影響を受けていないことを確認した後にのみ、`log` から `block` に移行します。
- 意図しない副作用を特定するために、QA、パフォーマンス、セキュリティテストチームを含めて、ルールを段階的に導入します。
- [ダッシュボードツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)を使用して、ルールの効果を定期的に確認および分析します。確認の頻度（毎日、毎週、毎月）は、サイトのトラフィック量とリスクプロファイルに合わせる必要があります。
- 新しい脅威インテリジェンス、トラフィックの動作、監査結果に基づいて、ルールを継続的に調整します。

## トラフィックフィルタールールのベストプラクティス

- アドビの[推奨される標準トラフィックフィルタールール](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)をベースラインとして使用します。これには、エッジ、接触チャネル保護、OFAC ベースの制限のルールが含まれます。
- 定期的にアラートとログを確認して、不正使用や誤った設定のパターンを特定します。
- アプリケーションのトラフィックパターンとユーザーの行動に基づいて、レート制限のしきい値を調整します。

  しきい値の選択方法のガイダンスについては、次の表を参照してください。

  | バリエーション | 値 |
  | :--------- | :------- |
  | 接触チャネル | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大接触チャネルリクエストの最大値を取得し、倍数で増やします。 |
  | Edge | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大 Edge リクエストの最大値を取得し、倍数で増やします。 |

  詳しくは、[しきい値の選択](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)の節も参照してください。

- `log` アクションが正当なトラフィックに影響を与えないことを確認した後にのみ、`block` アクションに移行します。

## WAF ルールのベストプラクティス

- まず、アドビの[推奨される WAF ルール](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)から開始します。これには、既知の不正な IP をブロックし、DDoS 攻撃を検出し、ボットの不正使用を軽減するためのルールが含まれます。
- `ATTACK` WAF フラグは、潜在的な脅威をアラートします。`block` に移行する前に、誤検出がないことを確認します。
- 推奨される WAF ルールが特定の脅威に対応していない場合は、アプリケーション固有の要件に基づいてカスタムルールを作成することを考慮します。[WAF フラグ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)の完全なリストについて詳しくは、ドキュメントを参照してください。

## ルールの実装

AEM as a Cloud Service でトラフィックフィルタールールと WAF ルールを実装する方法について説明します。

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="標準トラフィックフィルタールールを使用した AEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="標準トラフィックフィルタールールを使用した AEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="標準トラフィックフィルタールールを使用した AEM web サイトの保護">標準トラフィックフィルタールールを使用したAEM web サイトの保護</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される標準トラフィックフィルタールールを使用して、AEM web サイトを DoS 攻撃、DDoS 攻撃、ボットの不正使用から保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ルールを適用</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="WAF ルールを使用した AEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="WAF ルールを使用した AEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="WAF ルールを使用した AEM web サイトの保護">WAF ルールを使用した AEM web サイトの保護</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される web アプリケーションファイアウォール（WAF）ルールを使用して、DoS 攻撃、DDoS 攻撃、ボットの不正使用などの高度な脅威から AEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF をアクティブ化</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## その他のリソース

- [WAF ルールを含むトラフィックフィルタールール](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [AEM での DoS/DDoS 攻撃防止について](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [トラフィックフィルタールールを使用した DoS 攻撃と DDoS 攻撃のブロック](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)
