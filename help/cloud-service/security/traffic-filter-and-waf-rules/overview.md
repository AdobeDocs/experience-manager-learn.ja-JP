---
title: 概要 - AEM web サイトの保護
description: AEM as a Cloud Service の web アプリケーションファイアウォール（WAF）ルールのサブカテゴリを含むトラフィックフィルタールールを使用して、AEM web サイトを DoS 攻撃、DDoS 攻撃、悪意のあるトラフィックから保護する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 100%

---

# 概要 - AEM web サイトの保護

AEM as a Cloud Service の **web アプリケーションファイアウォール（WAF）**&#x200B;ルールのサブカテゴリを含む&#x200B;**トラフィックフィルタールール**&#x200B;を使用して、AEM web サイトをサービス拒否（DoS）攻撃、分散型サービス拒否（DDoS）攻撃、悪意のあるトラフィック、高度な攻撃から保護する方法について説明します。

また、標準トラフィックフィルタールールと WAF トラフィックフィルタールールの違い、使用するタイミング、アドビの推奨されるルールの使用を開始する方法についても説明します。

>[!IMPORTANT]
>
> WAFトラフィックフィルタールールには、追加の **WAF-DDoS 保護**&#x200B;または&#x200B;**拡張セキュリティ**&#x200B;のライセンスが必要です。Sites および Forms のお客様は、標準トラフィックフィルタールールをデフォルトで利用できます。


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## AEM as a Cloud Service のトラフィックセキュリティの概要

AEM as a Cloud Service では、統合された CDN レイヤーを活用して、web サイトの配信を保護および最適化します。CDN レイヤーの最も重要なコンポーネントの 1 つは、トラフィックルールを定義および適用する機能です。これらのルールは、パフォーマンスを犠牲にすることなく、不正使用、誤用、攻撃からサイトを保護するための保護シールドとして機能します。

トラフィックセキュリティは、稼動時間を維持し、機密性の高いデータを保護し、正当なユーザーに対してシームレスなエクスペリエンスを提供するために不可欠です。AEM では、次の 2 つのカテゴリのセキュリティルールを提供します。

- **標準トラフィックフィルタールール**
- **Web アプリケーションファイアウォール（WAF）トラフィックフィルタールール**

ルールセットは、一般的な web 脅威と高度な web 脅威を防御し、悪意のあるクライアントや不正なクライアントからのノイズを削減し、リクエストのログ記録、ブロック、パターン検出を通じて確認性を向上させるのに役立ちます。

## 標準および WAF トラフィックフィルタールールの違い

| 機能 | 標準トラフィックフィルタールール | WAF トラフィックフィルタールール |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 目的 | DoS 攻撃、DDoS 攻撃、スクレイピング、ボットアクティビティなどの不正使用を防止します | 高度な攻撃パターン（OWASP 上位 10 件など）を検出して対応します。また、ボットからも保護します |
| 例 | レート制限、ジオブロック、ユーザーエージェントフィルタリング | SQL インジェクション、XSS、既知の攻撃 IP |
| 柔軟性 | YAML 経由での高度な設定が可能 | 事前定義済みの WAF フラグを使用し、YAML 経由での高度な設定が可能 |
| 推奨されるモード | `log` モードで開始し、`block` モードに移行します | `ATTACK-FROM-BAD-IP` WAF フラグを `block` モード、`ATTACK` WAF フラグを `log` モードで開始し、両方を `block` モードに移行します |
| デプロイメント | YAML で定義し、Cloud Manager 設定パイプライン経由でデプロイします | `wafFlags` を使用して YAML で定義し、Cloud Manager 設定パイプライン経由でデプロイします |
| ライセンス | Sites および Forms ライセンスに含まれます | **WAF-DDoS 保護または拡張セキュリティのライセンスが必要です** |

標準トラフィックフィルタールールは、レート制限や特定の地域のブロックなどのビジネス固有のポリシーの適用と、IPアドレス、パス、ユーザーエージェントなどのリクエストのプロパティおよびヘッダーに基づくトラフィックのブロックに役立ちます。
一方、WAF トラフィックフィルタールールは、既知の web 悪用や攻撃ベクトルに対する包括的でプロアクティブな保護を提供し、誤検知（例：正当なトラフィックのブロック）を制限する高度なインテリジェンスを備えています。
両方のタイプのルールを定義するには、YAML 構文を使用します。詳しくは、[トラフィックフィルタールールの構文](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)を参照してください。

## 使用するタイミングと理由

**標準トラフィックフィルタールール**&#x200B;は、次の場合に使用します。

- IP レートスロットルなど、組織固有の制限を適用する必要がある。
- フィルタリングが必要な特定のパターン（例：悪意のある IP アドレス、地域、ヘッダー）を認識している。

**WAF トラフィックフィルタールール**&#x200B;は、次の場合に使用します。

- エキスパーのデータソースから収集された既知の悪意のある IP だけでなく、広い既知の攻撃パターン（例：インジェクション、プロトコルの不正使用）から、包括的で&#x200B;**プロアクティブな保護**&#x200B;が必要である。
- 正当なトラフィックをブロックする可能性を制限しながら、悪意のあるリクエストを拒否する必要がある。
- シンプルな設定ルールを適用して、一般的な脅威と高度な脅威に対する防御にかかる労力を制限する必要がある。

これらのルールを組み合わせることで、AEM as a Cloud Service のお客様がデジタルプロパティのセキュリティを確保するためにプロアクティブとリアクティブの両方の対策を講じることができる多層防御戦略が実現します。

## アドビの推奨されるルール

アドビでは、AEM サイトをすばやく保護できるように、標準トラフィックフィルタールールと WAF トラフィックフィルタールールの推奨されるルールを提供します。

- **標準トラフィックフィルタールール**（デフォルトで使用可能）：**CDN エッジ**、**接触チャネル**&#x200B;または制裁対象国からのトラフィックに対する DoS 攻撃、DDoS 攻撃、ボット攻撃などの一般的な不正使用シナリオに対処します。\
  以下に例を示します。
   - _CDN エッジ_&#x200B;で 1 秒あたり 500 件を超えるリクエストを行う IP のレート制限
   - _接触チャネル_&#x200B;で 1 秒あたり 100 件を超えるリクエストを行う IP のレート制限
   - 米国財務省外国資産管理室（OFAC）がリストした国からのトラフィックのブロック

- **WAF トラフィックフィルタールール**（アドオンライセンスが必要）：SQL インジェクション、クロスサイトスクリプティング（XSS）、他の web アプリケーション攻撃など、[OWASP 上位 10 件](https://owasp.org/www-project-top-ten/)の脅威を含む高度な脅威に対する追加の保護を提供します。
以下に例を示します。
   - 既知の不正な IP アドレスからのリクエストのブロック
   - 攻撃としてフラグ付けされた疑わしいリクエストのログ記録またはブロック

>[!TIP]
>
> アドビのセキュリティに関する専門知識と継続的な更新のメリットを活用するには、まず&#x200B;**アドビの推奨されるルール**&#x200B;を適用します。ビジネスに特定のリスクやエッジケースがある場合や、誤検知（正当なトラフィックのブロック）が発生した場合は、**カスタムルール**&#x200B;を定義するか、デフォルトのセットをニーズに合わせて拡張できます。

## 今すぐ始める

次の設定ガイドとユースケースに従って、AEM as a Cloud Service でトラフィックフィルタールール（WAF ルールを含む）を定義、デプロイ、テスト、分析する方法について説明します。これにより、アドビの推奨されるルールを自信を持って適用するための背景知識が得られます。

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
                        <a href="./setup.md" target="_self" rel="referrer" title="WAF ルールを含むトラフィックフィルタールールの設定方法">WAF ルールを含むトラフィックフィルタールールの設定方法</a>
                    </p>
                    <p class="is-size-6">WAF ルールを含むトラフィックフィルタールールの作成、デプロイ、テスト、結果の分析を設定する方法について説明します。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">今すぐ開始</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## アドビの推奨されるルール設定ガイド

このガイドでは、AEM as a Cloud Service 環境でアドビの推奨される標準トラフィックフィルタールールと WAF トラフィックフィルタールールを設定およびデプロイするための手順について段階的に説明します。

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
                    <p class="is-size-6">AEM as a Cloud Service でアドビの推奨される web アプリケーションファイアウォール（WAF）トラフィックフィルタールールを使用して、DoS 攻撃、DDoS 攻撃、ボットの不正使用などの高度な脅威から AEM web サイトを保護する方法について説明します。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF をアクティブ化</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 高度なユースケース

より高度なシナリオについて詳しくは、特定のビジネス要件に基づいてカスタムトラフィックフィルタールールを実装する方法を示す次のユースケースを参照してください。

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
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="機密性の高いリクエストの監視">機密性の高いリクエストの監視</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用して機密性の高いリクエストをログに記録し、監視する方法について説明します。</p>
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
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="アクセスの制限">アクセスの制限</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法について説明します。</p>
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
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="リクエストの標準化">リクエストの標準化</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service のトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法について説明します。</p>
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

- [WAF ルールを含むトラフィックフィルタールール](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
