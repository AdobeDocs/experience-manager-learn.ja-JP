---
title: WAF ルールを使用したAEM web サイトの保護
description: AEM as a Cloud ServiceでAdobeが推奨する Web Application Firewall （WAF）ルールを使用して、DoS、DDoS、ボットの不正使用などの高度な脅威からAEM web サイトを保護する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
badgeLicense: label="ライセンスが必要" type="positive" before-title="true"
jira: KT-18308
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 7%

---

# WAF ルールを使用したAEM web サイトの保護

AEM as a Cloud Serviceで _AEMが推奨する_ Web Application Firewall （WAF）ルール **を使用して、DoS、DDoS、ボットの不正使用などの高度な脅威からAdobe web サイトを保護する方法について説明** ます。

高度な攻撃の特徴は、高いリクエスト率、複雑なパターン、従来のセキュリティ対策を回避するための高度な技術の使用です。

>[!IMPORTANT]
>
> WAF トラフィックフィルタールールには、追加の **WAF-DDoS 保護** または **セキュリティの強化** ライセンスが必要です。 標準のトラフィックフィルタールールは、Sites およびFormsのお客様がデフォルトで使用できます。

## 学習目標

- Adobeが推奨するWAF ルールを確認します。
- ルールの結果を定義、デプロイ、テストおよび分析します。
- 結果に基づいてルールを絞り込むタイミングと方法を理解します。
- AEM アクションセンターを使用して、ルールによって生成されたアラートを確認する方法について説明します。

### 実装の概要

実装手順は次のとおりです。

- WAF ルールをAEM WKND プロジェクトの `/config/cdn.yaml` ファイルに追加する。
- 変更内容をコミットしてCloud Manager Git リポジトリにプッシュします。
- Cloud Manager設定パイプラインを使用して、変更内容をAEM環境にデプロイする。
- [Nikto](https://github.com/sullo/nikto/wiki) を使用して DDoS 攻撃をシミュレートし、ルールをテスト。
- AEMCS CDN ログと ELK ダッシュボードツールを使用した結果の分析。

## 前提条件

続行する前に、[ トラフィックフィルターとWAF ルールの設定方法 ](../setup.md) チュートリアルの説明に従って、必要な設定を完了していることを確認してください。 また、[AEM WKND サイトプロジェクト ](https://github.com/adobe/aem-guides-wknd) のクローンを作成して、AEM環境にデプロイしました。

## ルールの確認と定義

Adobeが推奨する Web アプリケーションファイアウォール（WAF）のルールは、AEM Web サイトを DoS、DDoS、ボットの不正使用などの高度な脅威から保護するために不可欠です。 高度な攻撃の多くは、高い要求率、複雑なパターン、および従来のセキュリティ対策を回避するための高度な技術（プロトコルベースまたはペイロードベースの攻撃）の使用によって特徴付けられます。

AEM WKND プロジェクトの `cdn.yaml` ファイルに追加される 3 つの推奨されるWAF ルールを確認しましょう。

### 1.既知の悪意のある IP からの攻撃をブロックする

この規則 **ブロック** は、悪意のあるものとしてフラグ付けされた IP アドレスから生成される、疑わしい *および* 要求の両方を示します。 これらの条件はどちらも満たされているので、誤検知（正当なトラフィックがブロックされる）のリスクは非常に低いと確信できます。 既知の不正な IP は、脅威インテリジェンスフィードやその他のソースに基づいて特定されます。

これらのリクエストを識別するには、`ATTACK-FROM-BAD-IP` WAF フラグを使用します。 いくつかのWAF フラグ [ ここにリストされています ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) を集計します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: attacks-from-bad-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: block
        wafFlags:
          - ATTACK-FROM-BAD-IP
```

### 2.任意の IP からの攻撃をグローバルに記録（および後でブロック）

このルール **ログ** は、IP アドレスが脅威インテリジェンスフィードに見つからない場合でも、潜在的な攻撃として識別される要求を記録します。

これらのリクエストを識別するには、`ATTACK` WAF フラグを使用します。 `ATTACK-FROM-BAD-IP` と同様、   複数のWAF フラグを集約します。

これらのリクエストは悪意のある可能性がありますが、IP アドレスは脅威インテリジェンスフィードで識別されないので、ブロックモードではなく `log` モードで開始することをお勧めします。 ログで誤検出を分析し、検証後は **ルールを `block` モードに切り替えてください**。

```yaml
...
    - name: attacks-from-any-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: log
        alert: true
        wafFlags:
          - ATTACK
```

または、ビジネス要件が、悪意のあるトラフィックを許可する可能性を避けたい場合は、すぐに `block` モードを使用することもできます。

これらの推奨されるWAFのルールは、既知の脅威や新しい脅威に対するセキュリティレイヤーを強化します。

![WKND WAFのルール ](../assets/use-cases/wknd-cdn-yaml-waf-rules.png)

## Adobeで推奨される最新のWAF ルールへの移行

`ATTACK-FROM-BAD-IP` および `ATTACK` WAFのフラグ（2025 年 7 月）が導入される前は、推奨されるWAFのルールは次のとおりです。 `SANS`、`TORNODE`、`NOUA` など、特定の条件に一致するリクエストをブロックするための特定のWAF フラグのリストが含まれていました。

```yaml
...
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
...
```

上記の規則は有効ですが、（ビジネス要件に合わせてこの規則をまだカスタマイズしていない場合は `ATTACK-FROM-BAD-IP``ATTACK` フラグと _WAF フラグを使用する新しい `wafFlags` 則に移行することをお勧めします_。

ベストプラクティスとの一貫性を保つために、新しいルールに移行するには、次の手順に従います。

- `cdn.yaml` ファイル内の既存のWAF ルールを確認します（上記の例に似ている場合があります）。 ビジネス要件に固有の `wafFlags` のカスタマイズがないことを確認します。

- 既存のWAF ルールを、`ATTACK-FROM-BAD-IP` フラグと `ATTACK` フラグを使用する新しいAdobe推奨WAF ルールに置き換えます。 すべてのルールがブロックモードになっていることを確認します。

以前に `wafFlags` をカスタマイズしていた場合は、これらの新しいルールに移行できますが、カスタマイズが改訂されたルールに取り込まれるように、慎重に移行する必要があります。

この移行により、WAFのルールをシンプル化しながら、高度な脅威に対する堅牢な保護を実現できます。 新しいルールは、より効果的で管理しやすくなるように設計されています。


## ルールのデプロイ

上記のルールをデプロイするには、次の手順に従います。

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- Cloud Manager設定パイプラインを使用して [ 以前に作成した ](../setup.md#deploy-rules-using-adobe-cloud-manager) 変更内容をAEM環境にデプロイします。

  ![Cloud Manager 設定パイプライン](../assets/use-cases/cloud-manager-config-pipeline.png)

## ルールのテスト

WAFのルールが効果を発揮しているかどうかを検証するため、脆弱性や設定ミスを検知するウェブサーバスキャナ [Nikto](https://github.com/sullo/nikto) を使って攻撃をシミュレーションします。 次のコマンドは、WAF ルールで保護されているAEM WKND web サイトに対して SQL 挿入攻撃をトリガーします。

```shell
$./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
```

![Nikto 攻撃シミュレーション](../assets/use-cases/nikto-attack.png)

攻撃シミュレーションについて詳しくは、[Nikto - スキャンの調整](https://github.com/sullo/nikto/wiki/Scan-Tuning)のドキュメントを参照してください。このドキュメントには、含めるまたは除外するテスト攻撃のタイプを指定する方法が記載されています。

## アラートを確認

トラフィックフィルタールールがトリガーされると、アラートが生成されます。 これらのアラートは、[AEM アクションセンター ](https://experience.adobe.com/aem/actions-center) で確認できます。

![WKND AEM アクションセンター ](../assets/use-cases/wknd-aem-action-center.png)

## 分析結果

トラフィックフィルタールールの結果を分析するには、AEMCS CDN ログと ELK ダッシュボードツールを使用します。 [CDN ログの取り込み ](../setup.md#ingest-cdn-logs) 設定セクションの手順に従って、CDN ログを ELK スタックに取り込みます。

次のスクリーンショットでは、AEM開発環境の CDN ログが ELK スタックに取り込まれていることがわかります。

![WKND CDN ログヘルプ ](../assets/use-cases/wknd-cdn-logs-elk-waf.png)

ELK アプリケーション内の **WAF ダッシュボード** には、
クライアント IP （cli_ip）、ホスト、URL、アクション（waf_action）、ルール名（waf_match）列のフラグが設定されたリクエストと対応する値。

![WKND WAF ダッシュボード ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged.png)

また、**WAF フラグ配布**&#x200B;パネルと&#x200B;**トップ攻撃**&#x200B;パネルには追加の詳細が表示されます。

![WKND WAF ダッシュボード ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![WKND WAF ダッシュボード ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-2.png)

![WKND WAF ダッシュボード ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-3.png)

### Splunk 統合

[Splunk ログの転送が有効になっている場合は](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)、新しいダッシュボードを作成してトラフィックパターンを分析できます。

Splunk でダッシュボードを作成するには、[AEMCS CDN ログ分析用の Splunk ダッシュボード](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)の手順に従います。

## ルールを調整するタイミングと方法

目的は、正当なトラフィックをブロックしないようにしながら、AEM web サイトを高度な脅威から保護することです。 推奨されるWAF ルールは、セキュリティ戦略の出発点として設計されています。

ルールを絞り込むには、次の手順を考慮します。

- **トラフィックパターンの監視**:CDN ログと ELK ダッシュボードを使用して、トラフィックパターンを監視し、トラフィックの異常やスパイクを特定します。 検出された攻撃の種類を把握するには、ELK ダッシュボードの _0}WAF フラグの配布_ および _上位の攻撃」パネルに注意してください。_
- **wafFlags の調整**：フラグが頻繁 `ATTACK` トリガーされるか、
攻撃ベクトルを微調整する必要があります。特定のWAF フラグを使用してカスタムルールを作成できます。 [WAF フラグ ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) の完全なリストについては、ドキュメントを参照してください。 最初に `log` モードで新しいカスタムルールを試すことを検討してください。
- **ブロッキングルールに移行**：トラフィックパターンを検証し、WAF フラグを調整したら、ブロッキングルールへの移行を検討できます。

## 概要

このチュートリアルでは、AEMが推奨する Web Application Firewall （WAF）ルールを使用して、DoS、DDoS、ボットの不正使用などの高度な脅威からAdobe Web サイトを保護する方法について説明します。

## ユースケース – 標準のルールを超えるもの

より高度なシナリオについては、特定のビジネス要件に基づいてカスタムトラフィックフィルタールールを実装する方法を示す、次のユースケースを参照できます。

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
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="機密性の高いリクエストの監視"> 機密リクエストの監視 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceのトラフィックフィルタールールを使用して機密リクエストをログに記録して監視する方法を説明します。</p>
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
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="アクセスの制限"> アクセスの制限 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceでトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法を説明します。</p>
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
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="リクエストの標準化"> リクエストの標準化 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceのトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法を説明します。</p>
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

- [ 推奨されるスタータールール ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)
- [WAF フラグの一覧 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)
