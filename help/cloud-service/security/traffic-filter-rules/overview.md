---
title: トラフィックフィルタールール（WAF ルールを含む）による web サイトの保護
description: Web アプリケーションファイアウォール（WAF）ルールのサブカテゴリを含む、トラフィックフィルタールールについて説明します。ルールを作成、デプロイおよびテストする方法。また、結果を分析して AEM sites を保護します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# トラフィックフィルタールール（WAF ルールを含む）による web サイトの保護

AEM as a Cloud Service（AEMCS）の **web アプリケーションファイアウォール（WAF）ルール**&#x200B;のサブカテゴリを含む、**トラフィックフィルタールール**&#x200B;について説明します。ルールを作成、デプロイおよびテストする方法についてお読みください。また、結果を分析して AEM sites を保護します。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概要

セキュリティ侵害のリスクを軽減することは、どの組織にとっても最優先事項です。AEMCS は、web サイトやアプリケーションを保護するための、WAF ルールを含むトラフィックフィルタールール機能を備えています。

トラフィックフィルタールールが[組み込み CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja) にデプロイされているので、リクエストが AEM インフラストラクチャに到達する前に評価されます。この機能を使用すると、web サイトのセキュリティを大幅に強化し、正当なリクエストのみを AEM インフラストラクチャにアクセスできるようにします。

このチュートリアルでは、WAF ルールを含むトラフィックフィルタールールの結果を作成、デプロイ、テスト、分析するプロセスについて説明します。

トラフィックフィルタールールについて詳しくは、[この記事](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ja)を参照してください。

>[!IMPORTANT]
>
> 「WAF ルール」と呼ばれるトラフィックフィルタールールのサブカテゴリには、WAF-DDoS 保護または拡張セキュリティライセンスが必要です。

トラフィックフィルタールールに関するフィードバックまたはご質問は、**aemcs-waf-adopter@adobe.com** にメールでお問い合わせください。

## 次の手順

この機能の[設定方法](./how-to-setup.md)を学習して、トラフィックフィルタールールを作成、デプロイおよびテストできるようにします。AEMCS CDN ログの結果を分析するための、**Elasticsearch、Logstash、 Kibana（ELK）**&#x200B;スタックダッシュボードツールの設定についてお読みください。


