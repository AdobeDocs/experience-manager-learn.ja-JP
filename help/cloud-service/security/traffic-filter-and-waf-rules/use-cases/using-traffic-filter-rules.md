---
title: 標準トラフィックフィルタールールを使用した AEM web サイトの保護
description: AEM as a Cloud Service でアドビの推奨される標準トラフィックフィルタールールを使用して、AEM web サイトをサービス拒否（DoS）攻撃およびボットの不正使用から保護する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
exl-id: 5e235220-82f6-46e4-b64d-315f027a7024
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1780'
ht-degree: 100%

---

# 標準トラフィックフィルタールールを使用した AEM web サイトの保護

AEM as a Cloud Service で&#x200B;_アドビが推奨する_**標準トラフィックフィルタールール**&#x200B;を使用して、AEM web サイトをサービス拒否（DoS）攻撃、分散型サービス拒否（DDoS）攻撃およびボットの不正使用から保護する方法について説明します。


>[!VIDEO](https://video.tv.adobe.com/v/3469396/?quality=12&learn=on)

## 学習目標

- アドビの推奨される標準トラフィックフィルタールールを確認します。
- ルールの結果を定義、デプロイ、テストおよび分析します。
- トラフィックパターンに基づいてルールを絞り込むタイミングと方法を理解します。
- AEM アクションセンターを使用して、ルールによって生成されたアラートを確認する方法を学びます。

### 実装の概要

実装手順には、次が含まれます。

- AEM WKND プロジェクトの `/config/cdn.yaml` ファイルに標準トラフィックフィルタールールを追加します。
- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。
- Cloud Manager 設定パイプラインを使用して、変更を AEM 環境にデプロイします。
- [Vegeta](https://github.com/tsenart/vegeta) を使用して DoS 攻撃をシミュレートして、ルールをテストします
- AEMCS CDN ログと ELK ダッシュボードツールを使用して、結果を分析します。

## 前提条件

続行する前に、[トラフィックフィルタールールと WAF ルールの設定方法](../setup.md)チュートリアルの説明に従って必要な基盤の設定が完了していることを確認します。また、[AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)のクローンを作成し、AEM 環境にデプロイしておきます。

## ルールの主なアクション

標準トラフィックフィルタールールの詳細に入る前に、これらのルールで実行される主なアクションについて理解しましょう。各ルールの `action` 属性は、条件が満たされた場合にトラフィックフィルターがどのように応答するかを定義します。アクションには、次が含まれます。

- **ログ**：ルールでは、監視と分析のためにイベントをログに記録します。これにより、トラフィックパターンを確認し、必要に応じてしきい値を調整できます。これは、`type: log` 属性で指定します。

- **アラート**：ルールでは、条件が満たされた場合にアラートをトリガーし、潜在的な問題を特定するのに役立ちます。これは、`alert: true` 属性で指定します。

- **ブロック**：ルールでは、条件が満たされた場合にトラフィックをブロックし、AEM サイトへのアクセスを防ぎます。これは、`action: block` 属性で指定します。

## ルールの確認と定義

アドビの推奨される標準トラフィックフィルタールールは、IP ベースのレート制限の超過などのイベントをログに記録することで、潜在的に悪意のあるトラフィックパターンを特定し、特定の国からのトラフィックをブロックするための基盤レイヤーとして機能します。これらのログは、チームがしきい値を検証し、正当なトラフィックを中断することなく、最終的に&#x200B;**ブロックモードへの移行**&#x200B;ルールについて十分な情報に基づいた決定を行うのに役立ちます。

AEM WKND プロジェクトの `/config/cdn.yaml` ファイルに追加する必要がある 3 つの標準トラフィックフィルタールールを確認しましょう。

- **Edge での DoS の防止**：このルールは、クライアント IP からの 1 秒あたりのリクエスト数（RPS）を監視することで、CDN エッジでの潜在的なサービス拒否（DoS）攻撃を検出します。
- **接触チャネルでの DoS の防止**：このルールは、クライアント IP からのフェッチリクエストを監視することで、接触チャネルでの潜在的なサービス拒否（DoS）攻撃を検出します。
- **OFAC 対象国のブロック**：このルールは、OFAC（米国財務省外国資産管理室）の規制対象となる特定の国からのアクセスをブロックします。

### &#x200B;1. Edge での DoS の防止

このルールでは、CDN で潜在的なサービス拒否（DoS）攻撃を検出すると&#x200B;**アラートを送信**&#x200B;します。このルールをトリガーする条件は、クライアントがエッジの CDN POP（ポイントオブプレゼンス）ごとに **1 秒あたり 100 件のリクエスト**（10 秒間の平均）を超えた場合です。

**すべて**&#x200B;のリクエストをカウントし、クライアント IP ごとにグループ化します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

`action` 属性は、条件が満たされた場合にイベントをログに記録し、アラートをトリガーするようにルールに指定します。したがって、正当なトラフィックをブロックすることなく、潜在的な DoS 攻撃を監視するのに役立ちます。ただし、トラフィックパターンを検証し、しきい値を調整したら、最終的にこのルールをブロックモードに移行することが目標です。

### &#x200B;2. 接触チャネルでの DoS の防止

このルールでは、接触チャネルで潜在的なサービス拒否（DoS）攻撃を検出すると&#x200B;**アラートを送信**&#x200B;します。このルールをトリガーする条件は、クライアントが接触チャネルのクライアント IP ごとに **1 秒あたり 100 件のリクエスト**（10 秒間の平均）を超えた場合です。

**フェッチ**（キャッシュバイパスリクエスト）をカウントし、クライアント IP ごとにグループ化します。

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

`action` 属性は、条件が満たされた場合にイベントをログに記録し、アラートをトリガーするようにルールに指定します。したがって、正当なトラフィックをブロックすることなく、潜在的な DoS 攻撃を監視するのに役立ちます。ただし、トラフィックパターンを検証し、しきい値を調整したら、最終的にこのルールをブロックモードに移行することが目標です。

### &#x200B;3. OFAC 対象国のブロック

このルールでは、[OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information) の規制対象となる特定の国からのアクセスをブロックします。
必要に応じて、国リストを確認および変更できます。

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

`action` 属性は、指定された国からのアクセスをブロックするようにルールに指定します。これにより、セキュリティリスクをもたらす可能性のある地域からの AEM サイトへのアクセスを防ぐことができます。

上記のルールを含む完全な `cdn.yaml` ファイルは、次のようになります。

![WKND CDN YAML ルール](../assets/use-cases/wknd-cdn-yaml-rules.png)

## ルールのデプロイ

上記のルールをデプロイするには、次の手順に従います。

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- [以前に作成した](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定パイプラインを使用して、変更を AEM 環境にデプロイします。

  ![Cloud Manager 設定パイプライン](../assets/use-cases/cloud-manager-config-pipeline.png)

## ルールのテスト

標準トラフィックフィルタールールの効果を確認するには、**CDN Edge** と&#x200B;**接触チャネル**&#x200B;の両方で、汎用性の高い HTTP 負荷テストツールである [Vegeta](https://github.com/tsenart/vegeta) を使用して、高いリクエストトラフィックをシミュレートします。

- Edge で DoS ルールをテストします（500 rps の制限）。次のコマンドは、Edge のしきい値（500 rps）を超える、1 秒あたり 200 リクエストを 15 秒間シミュレートします。

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Vegeta DoS 攻撃の Edge](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  上記のレポートでは、*100％*&#x200B;成功と _200_ ステータスコードが示されています。ルールは `log` と `alert` に設定されているので、リクエストは&#x200B;_ブロックされません_&#x200B;が、監視、分析、アラート生成のためにログに記録されます。

- 接触チャネルで DoS ルールをテストします（100 rps の制限）。次のコマンドは、接触チャネルのしきい値（100 rps）を超える、1 秒あたり 110 フェッチリクエストをシミュレートします。キャッシュバイパスリクエストをシミュレートするために、各リクエストがフェッチリクエストとして処理されるように、一意のクエリパラメーターを含む `targets.txt` ファイルを作成します。

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Vegeta DoS 攻撃の接触チャネル](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  上記のレポートでは、*100％*&#x200B;成功と _200_ ステータスコードが示されています。ルールは `log` と `alert` に設定されているので、リクエストは&#x200B;_ブロックされません_&#x200B;が、監視、分析、アラート生成のためにログに記録されます。

- 簡単にするために、ここでは OFAC ルールのテストは行いません。

## アラートの確認

トラフィックフィルタールールをトリガーすると、アラートが生成されます。これらのアラートは、[AEM アクションセンター](https://experience.adobe.com/aem/actions-center)で確認できます。

![WKND AEM アクションセンター](../assets/use-cases/wknd-aem-action-center.png)

## 結果の分析

トラフィックフィルタールールの結果を分析するには、AEMCS CDN ログと ELK ダッシュボードツールを使用します。[CDN ログの取り込み](../setup.md#ingest-cdn-logs)設定の節の指示に従って、CDN ログを ELK スタックに取り込みます。

次のスクリーンショットでは、AEM 開発環境の CDN ログが ELK スタックに取り込まれていることがわかります。

![WKND CDN ログの ELK](../assets/use-cases/wknd-cdn-logs-elk.png)

ELK アプリケーション内の **CDN トラフィックダッシュボード**&#x200B;には、DoS 攻撃のシミュレーション中に **Edge** と&#x200B;**接触チャネル**&#x200B;でのスパイクが表示されます。

_クライアント IP および POP あたりの Edge の RPS_ と&#x200B;_クライアント IP および POP あたりの接触チャネルの RPS_ の 2 つのパネルには、それぞれ Edge と接触チャネルでの 1 秒あたりのリクエスト数（RPS）が、クライアント IP とポイントオブプレゼンス（POP）ごとにグループ化されて表示されます。

![WKND CDN Edge トラフィックダッシュボード](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

また、CDNトラフィックダッシュボードの他のパネル（_上位のクライアント IP_、_上位の国_、_上位のユーザーエージェント_&#x200B;など）を使用して、トラフィックパターンを分析することもできます。これらのパネルは、潜在的な脅威を特定し、それに応じてトラフィックフィルタールールを調整するのに役立ちます。

### Splunk 統合

[Splunk ログの転送が有効になっている場合は](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)、新しいダッシュボードを作成してトラフィックパターンを分析できます。

Splunk でダッシュボードを作成するには、[AEMCS CDN ログ分析用の Splunk ダッシュボード](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)の手順に従います。

次のスクリーンショットは、IP ごとの最大接触チャネルリクエスト数とエッジリクエスト数を表示する Splunk ダッシュボードの例を示しています。これは、潜在的な DoS 攻撃を特定するのに役立ちます。

![Splunk ダッシュボード - IP ごとの最大接触チャネルリクエスト数とエッジリクエスト数](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## ルールを絞り込むタイミングと方法

目標は、正当なトラフィックのブロックを回避し、AEM サイトを潜在的な脅威から引き続き保護することです。標準トラフィックフィルタールールは、正当なトラフィックをブロックすることなく、脅威を警告し、ログに記録する（およびモードが切り替わった際に最終的にブロックする）ように設計されています。

ルールを絞り込むには、次の手順を考慮します。

- **トラフィックパターンを監視**：CDN ログと ELK ダッシュボードを使用して、トラフィックパターンを監視し、トラフィックの異常やスパイクを特定します。
- **しきい値を調整**：トラフィックパターンに基づいて、ルールのしきい値（レート制限の増減）を調整し、特定の要件に適合させます。例えば、正当なトラフィックがアラートをトリガーしたことに気付いた場合は、レート制限を増やしたり、グループ化を調整したりできます。
しきい値の選択方法のガイダンスについては、次の表を参照してください。

  | バリエーション | 値 |
  | :--------- | :------- |
  | 接触チャネル | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大接触チャネルリクエストの最大値を取得し、倍数で増やします。 |
  | Edge | **通常**&#x200B;のトラフィック条件（つまり、DDoS 時のレートではない）における IP/POP あたりの最大 Edge リクエストの最大値を取得し、倍数で増やします。 |

  詳しくは、[しきい値の選択](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)の節も参照してください。

- **ブロックルールに移行**：トラフィックパターンを検証し、しきい値を調整したら、ルールをブロックモードに移行する必要があります。

## 概要

このチュートリアルでは、AEM as a Cloud Service でアドビの推奨される標準トラフィックフィルタールールを使用して、AEM web サイトをサービス拒否（DoS）攻撃、分散型サービス拒否（DDoS）攻撃およびボットの不正使用から保護する方法について説明します。

## 推奨される WAF ルール

高度な手法を使用して従来のセキュリティ対策を回避する高度な脅威から AEM web サイトを保護するために、アドビの推奨される WAF ルールを実装する方法について説明します。

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="WAF トラフィックフィルタールールを使用した AEM web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="WAF トラフィックフィルタールールを使用した AEM web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="WAF トラフィックフィルタールールを使用した AEM web サイトの保護">WAF トラフィックフィルタールールを使用した AEM web サイトの保護</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される web アプリケーションファイアウォール（WAF）トラフィックフィルタールールを使用して、DoS 攻撃、DDoS 攻撃、ボットの不正使用などの高度な脅威から AEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF をアクティブ化</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## ユースケース - 標準ルール以外

より高度なシナリオについて詳しくは、特定のビジネス要件に基づいてカスタムトラフィックフィルタールールを実装する方法を示す次のユースケースを参照してください。

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="機密性の高いリクエストの監視" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="機密性の高いリクエストの監視"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="機密性の高いリクエストの監視">機密性の高いリクエストの監視</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用して機密性の高いリクエストをログに記録し、監視する方法について説明します。</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="アクセスの制限" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="アクセスの制限"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="アクセスの制限">アクセスの制限</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法について説明します。</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="リクエストの標準化" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="リクエストの標準化"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="リクエストの標準化">リクエストの標準化</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法について説明します。</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## その他のリソース

- [推奨されるスタータールール](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)
