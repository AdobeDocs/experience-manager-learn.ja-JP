---
title: CDN ログ分析ツール
description: アドビが提供する AEM Cloud Service CDN ログ分析ツールと、CDN パフォーマンスおよび AEM 実装の両方に関するインサイトを取得する上でどのように役立つかについて説明します。
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '442'
ht-degree: 100%

---

# CDN ログ分析ツール

アドビが提供する _AEM Cloud Service CDN ログ分析ツール_と、CDN のパフォーマンスおよび AEM 実装の両方に関するインサイトを取得する上でどのように役立つかについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## 概要

[AEM as a Cloud Service CDN ログ分析ツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)には、CDN ログのリアルタイム監視と分析のために、[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) または [ELK スタック](https://www.elastic.co/elastic-stack)と統合できる事前定義済みのダッシュボードが用意されています。

このツールを使用すると、リアルタイム監視とプロアクティブな問題検出を実現できます。これにより、コンテンツ配信の最適化と、サービス拒否（DoS）および分散型サービス拒否（DDoS）攻撃に対する適切なセキュリティ対策を確保します。

## 主な機能

- 効率化されたログ分析
- リアルタイム監視
- シームレスな統合
- ダッシュボードの目的
   - 潜在的なセキュリティ脅威の特定
   - エンドユーザーエクスペリエンスの高速化

## ダッシュボードの概要

ログ分析をすばやく開始するために、アドビでは Splunk と ELK スタックの両方に対応する事前定義済みダッシュボードを用意しています。

- **CDN キャッシュヒット率**：HIT、PASS、MISS のステータス別に、合計キャッシュヒット率とリクエストの合計数に関するインサイトを提供します。また、上位の HIT、PASS、MISS の URL も提供します。

  ![CDN キャッシュヒット率](assets/CHR-dashboard.png)

- **CDN トラフィックダッシュボード**：CDN および接触チャネルのリクエストレート、4xx および 5xx エラーレート、キャッシュされていないリクエストを介したトラフィックに関するインサイトを提供します。また、クライアント IP アドレスごとの 1 秒あたりの最大 CND および接触チャネルのリクエスト数と、CDN 設定を最適化する詳細なインサイトも提供します。

  ![CDN トラフィックダッシュボード](assets/Traffic-dashboard.png)

- **WAF ダッシュボード**：分析、フラグ付け、ブロックされたリクエストを通じてインサイトを提供します。また、WAF フラグ ID による上位攻撃、クライアント IP、国、ユーザーエージェントによる上位 100 名の攻撃者など、WAF の設定を最適化するためのさらなる洞察も提供します。

  ![WAF ダッシュボード](assets/WAF-Dashboard.png)

## Splunk 統合

[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) を活用し、Splunk インスタンスへの AEMCS ログ転送を有効にしている組織は、事前定義済みのダッシュボードをすばやく読み込むことができます。この設定により、高速なログ分析がしやすくなり、AEM 実装を最適化し、DOS 攻撃などのセキュリティ上の脅威を軽減するための実用的なインサイトが提供されます。

[AEMCS CDN ログ分析の Splunk ダッシュボード](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)ガイドの使用を開始できます。


## ELK 統合

Elasticsearch、Logstash、Kibana を含む [ELK スタック](https://www.elastic.co/elastic-stack)は、ログ分析のもう一つの強力なオプションです。これは、Splunk の設定機能やログ転送機能にアクセスできない組織に有益です。ELK スタックをローカルでセットアップするのは簡単です。このツールは Docker 作成ファイルを提供するので、すぐに開始することができます。次に、事前定義済みダッシュボードを読み込み、Adobe Cloud Manager を使用してダウンロードした CDN ログを取り込むことができます。

[AEMCS CDN ログ分析の ELK Docker コンテナ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis)ガイドの使用を開始できます。
