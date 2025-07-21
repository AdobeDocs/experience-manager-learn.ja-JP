---
title: WAF ルールを含むトラフィックフィルタールールの設定方法
description: WAF ルールを含むトラフィックフィルタールールを作成、デプロイ、テスト、および分析するための設定方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18306
thumbnail: null
exl-id: 0a738af8-666b-48dc-8187-9b7e6a8d7e1b
source-git-commit: b7f567da159865ff04cb7e9bd4dae0b140048e7d
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 14%

---

# WAF ルールを含むトラフィックフィルタールールの設定方法

Web アプリケーションファイアウォール（WAF）ルールを含むトラフィックフィルタールールについて **設定方法** を説明します。 このチュートリアルでは、後続のチュートリアルの基盤を設定します。ここでは、ルールを設定してデプロイし、その後、結果のテストと分析を行います。

設定プロセスを示すために、チュートリアルでは [AEM WKND サイトプロジェクト ](https://github.com/adobe/aem-guides-wknd) を使用します。

>[!VIDEO](https://video.tv.adobe.com/v/3469395/?quality=12&learn=on)

## 設定の概要

後続のチュートリアルの基盤には、次の手順が含まれます。

- _フォルダーのAEM プロジェクト内での_ ルールの作成 `config`
- Adobe Cloud Manager設定パイプラインを使用して _ルールをデプロイ_ します。
- Curl、Vegeta、Nikto などのツールを使用した _ルールのテスト_
- _結果の分析_ AEMCS CDN ログ分析ツールを使用

## AEM プロジェクトでのルールの作成

AEM プロジェクト内の **標準** および **WAF** トラフィックフィルタールールを定義するには、次の手順に従います。

1. AEM プロジェクトの最上位レベルで、`config` という名前のフォルダーを作成します。

2. `config` フォルダー内に、`cdn.yaml` という名前のファイルを作成します。

3. `cdn.yaml` で次のメタデータ構造を使用します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
```

![WKND AEM プロジェクトルールファイルとフォルダー](./assets/setup/wknd-rules-file-and-folder.png)

[ 次のチュートリアル ](#next-steps) では、実装の強固な基盤として、Adobeの **推奨される標準トラフィックフィルターとWAF ルール** を上記のファイルに追加する方法を学びます。

## Adobe Cloud Managerを使用したルールのデプロイ

ルールのデプロイの準備として、次の手順に従います。

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) にログインし、プログラムを選択します。

2. **プログラムの概要** ページで、**パイプライン** カードに移動し、「**+追加**」をクリックして新しいパイプラインを作成します。

   ![Cloud Manager パイプラインカード](./assets/setup/cloud-manager-pipelines-card.png)

3. パイプラインウィザードで、次の操作を行います。

   - **タイプ**：デプロイメントパイプライン
   - **パイプライン名**：Dev-Config

   ![Cloud Manager 設定パイプラインダイアログ](./assets/setup/cloud-manager-config-pipeline-step1-dialog.png)

4. Source コード設定：

   - **デプロイするコード**：ターゲットデプロイメント
   - **次を含む**：設定
   - **デプロイメント環境**：例：`wknd-program-dev`
   - **リポジトリ**:Git リポジトリ（`wknd-site` など）
   - **Git ブランチ**：作業ブランチ
   - **コードの場所**: `/config`

   ![Cloud Manager 設定パイプラインダイアログ](./assets/setup/cloud-manager-config-pipeline-step2-dialog.png)

5. パイプライン設定をレビューし、「**保存** をクリックします。

[ 次のチュートリアル ](#next-steps) では、パイプラインをAEM環境にデプロイする方法を説明します。

## ツールを使用したルールのテスト

標準のトラフィックフィルターとWAF ルールの効果をテストするには、様々なツールを使用してリクエストをシミュレートし、ルールがどのように応答するかを分析します。

次のツールがローカル コンピュータにインストールされていることを確認するか、手順に従ってツールをインストールします。

- [Curl](https://curl.se/)：リクエスト/応答フローをテストします。
- [Vegeta](https://github.com/tsenart/vegeta)：高いリクエスト負荷をシミュレートします（DoS テスト）。
- [Nikto](https://github.com/sullo/nikto/wiki)：脆弱性をスキャン。

次のコマンドを使用して、インストールを確認できます。

```shell
# Curl version check
$ curl --version

# Vegeta version check
$ vegeta -version

# Nikto version check
$ cd <PATH-OF-CLONED-REPO>/program
$ ./nikto.pl -Version
```

[ 次のチュートリアル ](#next-steps) では、これらのツールを使用して、高いリクエスト負荷と悪意のあるリクエストをシミュレートし、トラフィックフィルターとWAF ルールの効果をテストする方法を学びます。

## 分析結果

結果の分析を準備するには、次の手順に従います。

1. **AEMCS CDN ログ分析ツール** をインストールし、事前定義済みのダッシュボードを使用してパターンを視覚化および分析します。

2. Cloud Manager UI からログをダウンロードして、**CDN ログの取り込み** を実行します。 または、ログを、Splunk やElasticsearchなどのサポートされているホストされたログの宛先に直接転送することもできます。

### AEMCS CDN ログ分析ツール

トラフィックフィルターとWAF ルールの結果を分析するには、**AEMCS CDN Log Analysis Tooling** を使用することができます。 このツールは、AEMCS CDN から収集されたログを活用して CDN トラフィックとWAF アクティビティを視覚化する、事前定義済みのダッシュボードを提供します。

AEMCS CDN Log Analysis Tooling は、**ELK** （Elasticsearch、Logstash、Kibana）と **Splunk** の 2 つの観測プラットフォームをサポートしています。

ログ転送機能を使用して、ホストされている ELK または Splunk ログサービスにログをストリーミングでき、ダッシュボードをインストールして、標準のトラフィックフィルターとWAFのトラフィックフィルタールールを視覚化して分析できます。 ただし、このチュートリアルでは、コンピュータにインストールされたローカル ELK インスタンス上にダッシュボードを設定します。

1. [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) リポジトリをクローンします。

2. [ELK Docker コンテナセットアップガイド ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) に従って、ELK スタックをローカルにインストールして設定します。

3. ELK ダッシュボードを使用すると、IP リクエスト、ブロックされたトラフィック、URI パターン、セキュリティアラートなどの指標を調べることができます。

   ![ELK トラフィックフィルタールールダッシュボード](./assets/setup/elk-dashboard.png)

>[!NOTE]
> 
> ログがまだ AEMCS CDN から取り込まれていない場合、ダッシュボードは空で表示されます。

### CDN ログの取得

CDN ログを ELK スタックに取り込むには、次の手順に従います。

- [Cloud Manager](https://my.cloudmanager.adobe.com/) の&#x200B;**環境**&#x200B;カードから、AEMCS **パブリッシュ**&#x200B;サービスの CDN ログをダウンロードします。

  ![Cloud Manager CDN ログのダウンロード](./assets/setup/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新しいリクエストが CDN ログに表示されるまでに最大 5 分かかる場合があります。

- ダウンロードしたログファイル（例：以下のスクリーンショットの `publish_cdn_2025-06-06.log`）を Elastic ダッシュボードツールプロジェクトの `logs/dev` フォルダーにコピーします。

  ![ELK ツールログフォルダー](./assets/setup/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Elastic ダッシュボードツールページを更新します。
   - 上部の「**グローバルフィルター**」セクションで、`aem_env_name.keyword` フィルターを編集し、`dev` 環境値を選択します。

     ![ELK ツールグローバルフィルター](./assets/setup/elk-tool-global-filter.png)

   - 時間間隔を変更するには、右上隅にあるカレンダーアイコンをクリックし、目的の時間間隔を選択します。

- [ 次のチュートリアル ](#next-steps) では、ELK スタック内の事前定義済みダッシュボードを使用して、標準トラフィックフィルターとWAF トラフィックフィルタールールの結果を分析する方法を学びます。

  ![ELK ツールの事前定義済みダッシュボード ](./assets/setup/elk-tool-pre-built-dashboards.png)

## 概要

AEM as a Cloud ServiceでWAF ルールを含むトラフィックフィルタールールを実装するための基盤の設定に成功しました。 設定ファイル構造、デプロイメント用のパイプラインおよび結果のテストと分析のための準備ツールを作成しました。

## 次の手順

次のチュートリアルを使用して、Adobeの推奨ルールを実装する方法を説明します。

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
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="AEM トラフィックフィルタールールを使用したWAF web サイトの保護" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="AEM トラフィックフィルタールールを使用したWAF web サイトの保護"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="AEM トラフィックフィルタールールを使用したWAF web サイトの保護">AEM トラフィックフィルタールールを使用したWAF web サイトの保護 </a>
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

Adobeで推奨される標準トラフィックフィルターおよびWAF ルールの他に、高度なシナリオを実装して特定のビジネス要件を達成することもできます。 次のようなシナリオがあります。

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
