---
title: トラフィックフィルタールール（WAF ルールを含む）による Web サイトの保護
description: Web アプリケーションファイアウォール (WAF) ルールのサブカテゴリを含む、トラフィックフィルタールールについて説明します。 ルールを作成、デプロイおよびテストする方法。 また、結果を分析してAEMサイトを保護します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: bca52c7543b35fc20a782dfd3f2b2dc81bee4cde
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 4%

---


# トラフィックフィルタールール（WAF ルールを含む）による Web サイトの保護

詳細 **トラフィックフィルタールール**（次のサブカテゴリを含む） **Web アプリケーションファイアウォール (WAF) ルール** (AEMas a Cloud Service(AEMCS)) の ルールの作成、デプロイ、テストの方法についてお読みください。 また、結果を分析してAEMサイトを保護します。

## 概要

セキュリティ侵害のリスクを軽減することは、どの組織にとっても最も優先事項です。 AEMCS には、Web サイトやアプリケーションを保護するためのトラフィックフィルタールール機能（WAF ルールを含む）が用意されています。

トラフィックフィルタールールが [組み込み CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja) リクエストがAEMインフラストラクチャに到達する前に評価されます。 この機能を使用すると、Web サイトのセキュリティを大幅に強化し、正当なリクエストのみがAEMインフラストラクチャにアクセスできるようにすることができます。

このチュートリアルでは、WAF ルールを含むトラフィックフィルタールールの作成、デプロイ、テスト、および結果の分析のプロセスについて説明します。

>[!IMPORTANT]
>
> 「WAF ルール」と呼ばれるトラフィックフィルタルールのサブカテゴリには、WAF-DoS 保護ライセンスが必要です。


## 次の手順

学ぶ [設定方法](./how-to-setup.md) トラフィックフィルタールールを作成、デプロイおよびテストするための機能。 詳しくは、 **Elasticsearch、ログスタッシュ、きばな (ELK)** AEMCS CDN ログの結果を分析するためのスタックダッシュボードツール。



