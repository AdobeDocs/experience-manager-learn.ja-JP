---
title: トラフィックフィルタールール（WAF ルールを含む）による Web サイトの保護
description: Web アプリケーションファイアウォール (WAF) ルールのサブカテゴリを含む、トラフィックフィルタールールについて説明します。 ルールを作成、デプロイおよびテストする方法。 また、結果を分析してAEMサイトを保護します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 3%

---

# トラフィックフィルタールール（WAF ルールを含む）による Web サイトの保護

詳細 **トラフィックフィルタールール**（次のサブカテゴリを含む） **Web アプリケーションファイアウォール (WAF) ルール** (AEMas a Cloud Service(AEMCS)) の ルールの作成、デプロイ、テストの方法についてお読みください。 また、結果を分析してAEMサイトを保護します。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概要

セキュリティ侵害のリスクを軽減することは、どの組織にとっても最も優先事項です。 AEMCS は、Web サイトやアプリケーションを保護するための、WAF ルールを含むトラフィックフィルタールール機能を備えています。

トラフィックフィルタールールが [組み込み CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja) リクエストがAEMインフラストラクチャに到達する前に評価されます。 この機能を使用すると、Web サイトのセキュリティを大幅に強化し、正当なリクエストのみがAEMインフラストラクチャにアクセスできるようにすることができます。

このチュートリアルでは、WAF ルールを含むトラフィックフィルタールールの結果を作成、デプロイ、テスト、分析するプロセスについて説明します。

トラフィックフィルタールールについて詳しくは、 [この記事](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> 「WAF ルール」と呼ばれるトラフィックフィルタルールのサブカテゴリには、WAF-DoS 保護または拡張セキュリティライセンスが必要です。

フィードバックをいただくか、E メールでトラフィックフィルタールールに関するご質問をお寄せください **aemcs-waf-adopter@adobe.com**.

## 次の手順

学ぶ [設定方法](./how-to-setup.md) トラフィックフィルタールールを作成、デプロイおよびテストするための機能。 詳しくは、 **Elasticsearch、ログスタッシュ、きばな (ELK)** AEMCS CDN ログの結果を分析するためのスタックダッシュボードツール。


