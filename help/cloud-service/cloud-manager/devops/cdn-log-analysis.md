---
title: CDN ログ分析ツール
description: Adobeが提供するAEM Cloud Service CDN ログ分析ツールと、CDN パフォーマンスおよびAEM実装の両方に関するインサイトを得るのにどのように役立つかについて説明します。
version: Cloud Service
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
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 1%

---

# CDN ログ分析ツール

Adobeが提供する _AEM Cloud Service CDN ログ分析ツール_ と、CDN のパフォーマンスとAEM実装の両方に関するインサイトを得るのにどのように役立つかについて説明します。
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## 概要

[AEM as a Cloud Service CDN ログ分析ツール ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) には、CDN ログのリアルタイムな監視と分析のために、[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) または [ELK スタック ](https://www.elastic.co/elastic-stack) と統合できる事前定義済みのダッシュボードが用意されています。

このツールを使用すると、リアルタイムの監視とプロアクティブな問題検出を実現できます。 これにより、コンテンツ配信の最適化と、サービス拒否（DoS）および分散型サービス拒否（DDoS）攻撃に対する適切なセキュリティ対策が確保されます。

## 主な機能

- 効率化されたログ分析
- リアルタイム監視
- シームレスな統合
- のダッシュボード
   - 潜在的なセキュリティの脅威を特定する
   - エンドユーザーエクスペリエンスの高速化

## ダッシュボードの概要

ログ分析をすばやく開始するために、Adobeでは Splunk と ELK スタックの両方に対応する事前定義済みダッシュボードを提供しています。

- **CDN キャッシュヒット率**：キャッシュヒット率の合計と、リクエストの合計数（ヒット、合格、ミスのステータス別）に関するインサイトを提供します。 また、上位のヒット、合格およびミス URL も提供します。

  ![CDN キャッシュヒット率 ](assets/CHR-dashboard.png)

- **CDN トラフィックダッシュボード**:CDN とオリジンリクエスト率、4xx と 5xx のエラー率、およびキャッシュされていないリクエストを使用してトラフィックに関するインサイトを提供します。 また、クライアント IP アドレスあたりの 1 秒あたりの最大 CND およびオリジンリクエスト数と、CDN 設定を最適化するためのより多くのインサイトを提供します。

  ![CDN トラフィックダッシュボード ](assets/Traffic-dashboard.png)

- **WAF ダッシュボード**：分析済み、フラグ付き、ブロックされたリクエストを通じてインサイトを提供します。 また、WAF フラグ ID による上位攻撃、クライアント IP、国、ユーザーエージェントによる上位 100 名の攻撃者、および WAF 設定を最適化するためのより多くのインサイトを提供します。

  ![WAF ダッシュボード ](assets/WAF-Dashboard.png)

## Splunk 統合

[Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) を活用し、Splunk インスタンスへの AEMCS ログ転送を有効にしている組織の場合は、事前定義済みのダッシュボードをすばやく読み込むことができます。 この設定により、高速なログ分析が容易になり、AEMの実装を最適化し、DOS 攻撃などのセキュリティ上の脅威を軽減するための実用的なインサイトが提供されます。

AEMCS CDN ログ分析の [Splunk ダッシュボード ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis) ガイドの使用を開始できます。


## ELK 統合

Elasticsearch、Logstash、Kibana を含む [ELK スタック ](https://www.elastic.co/elastic-stack) は、ログ分析のもう一つの強力なオプションです。 これは、Splunk のセットアップ機能やログ転送機能にアクセスできない組織で役立ちます。 ELK スタックをローカルでセットアップするのは簡単です。このツールは Docker 作成ファイルを提供するので、すぐに使い始めることができます。 次に、事前定義済みダッシュボードを読み込み、Adobe Cloud Managerを使用してダウンロードした CDN ログを取り込むことができます。

AEMCS CDN ログ分析用 [ELK Docker コンテナ ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis) ガイドの使用を開始できます。
