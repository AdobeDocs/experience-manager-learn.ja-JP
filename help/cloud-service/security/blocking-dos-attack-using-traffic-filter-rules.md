---
title: トラフィックフィルタールールを使用した DoS 攻撃、DDoS 攻撃および高度な攻撃のブロック
description: AEM as a Cloud Service のトラフィックフィルタールールを使用して、DoS 攻撃、DDoS 攻撃および高度な攻撃をブロックする方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '609'
ht-degree: 100%

---

# トラフィックフィルタールールを使用した DoS 攻撃、DDoS 攻撃および高度な攻撃のブロック

AEM as a Cloud Service（AEMCS）が管理する CDN の&#x200B;**トラフィックフィルタールール**&#x200B;を使用して、サービス拒否（DoS）攻撃、分散型サービス拒否（DDoS）攻撃および高度な攻撃をブロックする方法について説明します。

これらの攻撃は、CDN および潜在的に AEM パブリッシュサービス（オリジン）でトラフィックの急増を引き起こし、サイトの応答性と可用性に影響を与える可能性があります。

この記事では、AEM web サイトのデフォルトの保護機能の概要と、顧客設定を通じてこれらの保護を拡張する方法について説明します。また、トラフィックパターンを分析し、標準トラフィックフィルタールールを設定してこれらの攻撃をブロックする方法についても説明します。

## AEM as a Cloud Service でのデフォルトの保護

AEM web サイトのデフォルトの DDoS 保護機能を説明します。

- **キャッシュ：**&#x200B;適切なキャッシュポリシーを使用すると、CDN により、ほとんどのリクエストが接触チャネルに送信されパフォーマンスが低下するのを防止できるので、DDoS 攻撃の影響は限定されます。
- **自動スケーリング：** AEM オーサーサービスとパブリッシュサービスは、トラフィックの急増に対処するために自動スケーリングされますが、トラフィックの突然の大量増加による影響を受ける可能性があります。
- **ブロック：** Adobe CDN は、CDN PoP（ポイントオブプレゼンス）ごとに特定の IP アドレスからアドビが定義したレートを超える場合、接触チャネルへのトラフィックをブロックします。
- **アラート：**&#x200B;トラフィックが特定のレートを超えた場合、アクションセンターは接触チャネルトラフィックスパイクのアラート通知を送信します。このアラートは、特定の CDN PoP へのトラフィックが IP アドレスごとの&#x200B;_アドビ定義_&#x200B;リクエストレートを超えると発生します。詳しくは、[トラフィックフィルタールールのアラート](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)を参照してください。

これらのビルトインの保護は、DDoS 攻撃のパフォーマンスへの影響を最小限に抑える、組織の能力のベースラインとして考える必要があります。Web サイトごとにパフォーマンス特性が異なり、アドビが定義したレート制限に達する前にパフォーマンスが低下する可能性があるので、_顧客設定_&#x200B;を通じてデフォルトの保護を拡張することをお勧めします。

## トラフィックフィルタールールによる保護の拡張

DDoS 攻撃から web サイトを保護するために、顧客が実行できる追加の推奨対策をいくつか見てみましょう。

- アドビの推奨される[標準トラフィックフィルタールール](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)を実装し、疑わしい動作をログに記録してアラートすることで、潜在的に悪意のあるトラフィックパターンを特定します。
- **WAF-DDoS 保護**&#x200B;または&#x200B;**拡張セキュリティ**&#x200B;アドオンを使用し、アドビの推奨される [WAF トラフィックフィルタールール](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)を実装して、高度なプロトコルやペイロードベースの手法を使用した攻撃を含む高度な攻撃に対して防御します。
- 不要なクエリパラメーターを無視するように[リクエスト変換](./traffic-filter-and-waf-rules/how-to/request-transformation.md)を設定して、キャッシュカバレッジを増やします。

## 今すぐ始める

攻撃をブロックするアドビの推奨されるルールを設定するには、次のチュートリアルを参照してください。

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="WAF ルールを含むトラフィックフィルタールールの設定方法" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="WAF ルールを含むトラフィックフィルタールールの設定方法"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="WAF ルールを含むトラフィックフィルタールールの設定方法">WAF ルールを含むトラフィックフィルタールールの設定方法</a>
                    </p>
                    <p class="is-size-6">WAF ルールを含むトラフィックフィルタールールの作成、デプロイ、テスト、結果の分析を設定する方法について説明します。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">今すぐ開始</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="標準トラフィックフィルタールールを使用した AEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="標準トラフィックフィルタールールを使用した AEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="標準トラフィックフィルタールールを使用した AEM web サイトの保護">標準トラフィックフィルタールールを使用したAEM web サイトの保護</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される標準トラフィックフィルタールールを使用して、AEM web サイトを DoS 攻撃、DDoS 攻撃、ボットの不正使用から保護する方法について説明します。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ルールを適用</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="WAF トラフィックフィルタールールを使用した AEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="WAF トラフィックフィルタールールを使用した AEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="WAF トラフィックフィルタールールを使用した AEM web サイトの保護">WAF トラフィックフィルタールールを使用した AEM web サイトの保護</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される web アプリケーションファイアウォール（WAF）トラフィックフィルタールールを使用して、DoS 攻撃、DDoS 攻撃、ボットの不正使用などの高度な脅威から AEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF をアクティブ化</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
