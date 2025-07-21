---
title: 概要 – AEM Web サイトの保護
description: AEM as a Cloud Serviceの Web アプリケーションファイアウォール（AEM）ルールのサブカテゴリを含むトラフィックフィルタールールを使用して、DoS、DDoS および悪意のあるトラフィックからWAF web サイトを保護する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---

# 概要 – AEM Web サイトの保護

AEMの **Web Application Firewall （WAF）** ルールのサブカテゴリを含む **トラフィックフィルタールール** を使用して、DoS、分散型 DoS （Denial of Service）、悪意のあるトラフィックおよび高度な攻撃からAEM as a Cloud Service web サイトを保護する方法について説明します。

また、標準トラフィックフィルターとWAF トラフィックフィルタールールの違い、それらを使用するタイミング、Adobeが推奨するルールの使用を開始する方法についても説明します。

>[!IMPORTANT]
>
> WAF トラフィックフィルタールールには、追加の **WAF-DDoS 保護** または **セキュリティの強化** ライセンスが必要です。 標準のトラフィックフィルタールールは、Sites およびFormsのお客様がデフォルトで使用できます。


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## AEM as a Cloud Serviceのトラフィックセキュリティの概要

AEM as a Cloud Serviceでは、統合 CDN レイヤーを活用して、web サイトの配信を保護および最適化します。 CDN レイヤーの最も重要なコンポーネントの 1 つは、トラフィックルールを定義および適用する機能です。 これらのルールは、パフォーマンスを犠牲にすることなく、サイトを乱用、誤用、攻撃から保護するための保護シールドとして機能します。

トラフィックのセキュリティは、稼働時間を維持し、機密データを保護し、正当なユーザーにシームレスなエクスペリエンスを提供するために不可欠です。 AEMには、次の 2 つのカテゴリのセキュリティルールがあります。

- **標準トラフィックフィルタールール**
- **Web アプリケーションファイアウォール（WAF）トラフィックフィルタールール**

このルールセットは、一般的で高度な web 脅威の防止、悪意のあるクライアントや不適切な動作をするクライアントからのノイズの低減、リクエストのロギング、ブロック、パターン検出による観察性の向上に役立ちます。

## 標準とWAFのトラフィックフィルタールールの違い

| 機能 | 標準トラフィックフィルタールール | WAF トラフィックフィルタールール |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 目的 | DoS、DDoS、スクレーピング、ボットアクティビティなどの乱用を防ぐ | ボットからも保護される高度な攻撃パターン（OWASPトップ 10 など）を検出して対処します。 |
| 例 | レート制限、ジオブロック、ユーザーエージェントフィルタリング | SQL インジェクション、XSS、既知の攻撃 IP |
| 柔軟性 | YAML 経由での高度な設定可能 | 事前定義済みのWAF フラグを使用し、YAML 経由で高度に設定可能 |
| 推奨モード | `log` モードで開始してから、`block` モードに移動します | WAF フラグの `block` モード `ATTACK-FROM-BAD-IP`WAF フラグの `log` モードから始めて、両方 `ATTACK` フラグの `block` モードに移行します |
| デプロイメント | YAML で定義され、Cloud Manager設定パイプラインを介してデプロイされます | `wafFlags` を使用して YAML で定義し、Cloud Manager設定パイプラインを介してデプロイします |
| ライセンス | Sites およびForms ライセンスに含まれる | **WAF-DDoS Protection または Enhanced Security のライセンスが必要** |

標準のトラフィックフィルタールールは、レート制限またはブロック特定の領域などのビジネス固有のポリシーを適用したり、IP アドレス、パス、ユーザーエージェントなどのリクエストプロパティおよびヘッダーに基づくトラフィックをブロックしたりするのに役立ちます。
一方、WAFのトラフィックフィルタールールは、既知の web 攻撃および攻撃ベクトルに対して包括的でプロアクティブな保護機能を提供し、誤検出を制限する高度なインテリジェンスを備えています（例：正当なトラフィックをブロックする）。
両方のタイプのルールを定義するには、YAML 構文を使用します。詳しくは、[ トラフィックフィルタールールの構文 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax) を参照してください。

## これらを使用するタイミングと理由

**標準のトラフィックフィルタールールの使用** は、次の場合に行います。

- IP レートスロットルなど、組織固有の制限を適用する場合。
- フィルタリングが必要な特定のパターン（悪意のある IP アドレス、地域、ヘッダーなど）を認識している。

**WAF トラフィックフィルタールールの使用** は次の場合に行います。

- エキスパートデータソースから収集された、既知の攻撃パターン（インジェクション、プロトコルの不正使用など）や、既知の悪意のある IP から包括的な **プロアクティブな保護** を得る必要があります。
- 正当なトラフィックをブロックする可能性を制限しながら、悪意のあるリクエストを拒否する場合。
- シンプルな設定ルールを適用することで、一般的で高度な脅威から保護する労力を制限する必要があります。

これらのルールを組み合わせることで、AEM as a Cloud Serviceのお客様は、デジタルプロパティを保護する際に、プロアクティブかつリアクティブな対策を講じることができます。

## Adobeが推奨するルール

Adobeには、標準トラフィックフィルターおよびWAF トラフィックフィルタールールの推奨ルールが用意されており、AEM サイトをすばやく保護するのに役立ちます。

- **標準トラフィックフィルタールール** （デフォルトで使用可能）:**CDN Edge**、**接触チャネル**、または制裁対象国からのトラフィックに対する DoS、DDoS およびボット攻撃などの一般的な乱用シナリオに対処します。\
  以下に例を示します。
   - （CDN エッジで _1 秒あたり 500 件を超えるリクエストを行う IP のレート制限_
   - （接触チャネルで _1 秒あたり 100 件を超えるリクエストを行う IP のレート制限_
   - 外国Assets管理庁（OFAC）の上場国からのトラフィックのブロック

- **WAF トラフィックフィルタールール** （アドオンライセンスが必要）: SQL インジェクション、クロスサイトスクリプティング（XSS） [ その他の web アプリケーション攻撃などの、](https://owasp.org/www-project-top-ten/)OWASP トップ 10} の高度な脅威に対する追加の保護を提供します。
以下に例を示します。
   - 既知の不正な IP アドレスからの要求をブロックしています
   - 攻撃のフラグが設定された疑わしい要求のログ記録またはブロック

>[!TIP]
>
> まず、Adobeのセキュリティに関する専門知識と継続的な更新を活用するために **Adobeが推奨するルール } を適用します。**&#x200B;ビジネスに特定のリスクやエッジケースがある場合、または誤検出（正当なトラフィックのブロック）が発生した場合は、**カスタムルール** を定義するか、デフォルトのセットをニーズに合わせて拡張できます。

## 今すぐ始める

次の設定ガイドとユースケースに従って、AEM as a Cloud ServiceでWAF ルールを含むトラフィックフィルタールールを定義、デプロイ、テスト、分析する方法を説明します。 これにより背景知識が習得でき、後で自信を持ってAdobeが推奨するルールを適用できます。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="WAF ルールを含むトラフィックフィルタールールの設定方法" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="WAF ルールを含むトラフィックフィルタールールの設定方法"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="WAF ルールを含むトラフィックフィルタールールの設定方法">WAF ルールを含むトラフィックフィルタールールの設定方法 </a>
                    </p>
                    <p class="is-size-6">WAF ルールを含むトラフィックフィルタールールを作成、デプロイ、テスト、および分析するための設定方法について説明します。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> 今すぐ開始 </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobeが推奨するルール設定ガイド

このガイドでは、AEM as a Cloud Service環境でAdobe推奨の標準トラフィックフィルターおよびWAF トラフィックフィルタールールを設定してデプロイする手順を順を追って説明します。

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
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
                    <p class="is-size-6">AEM as a Cloud ServiceでAdobeが推奨する Web Application Firewall （AEM）トラフィックフィルタールールを使用して、DoS、DDoS、ボットの不正使用などの高度な脅威からWAF web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAFのアクティブ化 </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 高度なユースケース

より高度なシナリオについては、特定のビジネス要件に基づいてカスタムトラフィックフィルタールールを実装する方法を示す、次のユースケースを参照できます。

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="機密性の高いリクエストの監視" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="機密性の高いリクエストの監視"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="機密性の高いリクエストの監視"> 機密リクエストの監視 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceのトラフィックフィルタールールを使用して機密リクエストをログに記録して監視する方法を説明します。</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="アクセスの制限" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="アクセスの制限"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="アクセスの制限"> アクセスの制限 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceでトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法を説明します。</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="リクエストの標準化" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="リクエストの標準化"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="リクエストの標準化"> リクエストの標準化 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Serviceのトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法を説明します。</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## その他のリソース

- [WAFルールを含むトラフィックフィルタールール ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
