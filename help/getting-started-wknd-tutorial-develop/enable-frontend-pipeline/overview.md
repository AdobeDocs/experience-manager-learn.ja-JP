---
title: 標準 AEM プロジェクトアーキタイプのフロントエンドパイプラインの有効化
description: 標準 AEM プロジェクトのフロントエンドパイプラインを有効にして、CSS、JavaScript、フォント、アイコンなどの静的リソースのデプロイを迅速化する方法を説明します。また、AEM のフルスタックバックエンド開発からフロントエンド開発を切り離す方法も説明します。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 100%

---

# 標準 AEM プロジェクトアーキタイプのフロントエンドパイプラインの有効化{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用して作成された [AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)（別名、標準 AEM プロジェクト）を有効にして、CSS、JavaScript、フォント、アイコンなどのフロントエンドリソースをフロントエンドパイプラインを使用してデプロイし、開発からデプロイメントまでのサイクルを高速化する方法を説明します。AEM のフルスタックバックエンド開発からフロントエンド開発を切り離す方法も説明します。また、これらのフロントエンドリソースが AEM リポジトリから&#x200B;__ではなく__ CDN から提供される方法、つまり配信パラダイムの変化についても説明します


`ui.frontend` アーティファクトの作成とビルトインの CDN へのデプロイのみを行う新しいフロントエンドパイプラインが Adobe Cloud Manager で作成され、AEM にその場所を通知します。AEM で、web ページの HTML 生成時に、`<link>` および `<script>` タグが `href` 属性値のこのアーティファクトの場所を参照します。

ただし、WKND Sites AEM プロジェクトの変換後は、フロントエンド開発者は、独自のデプロイメントパイプラインを持つ、AEM 上のフルスタックバックエンド開発とは別に、または並行して作業を行うことができます。

>[!IMPORTANT]
>
>一般に、フロントエンドパイプラインは [AEM クイックサイト作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=ja)と併用されます。詳しくは、関連チュートリアルの [AEM Sites の基本を学ぶ - クイックサイト作成](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ja)を参照してください。そのため、このチュートリアルと関連ビデオには、上記への言及が含まれます。これは、微妙な違いにしっかりと注意を向けさせるためです。また、重要な概念を説明するために直接的または間接的な比較を行っています。


関連する[マルチステップチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ja)では、クイックサイト作成機能を使用して、架空のライフスタイルブランド WKND の AEM サイトを実装する方法について順を追って説明します。 [テーマ設定ワークフロー](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=ja)を精査して、フロントエンドパイプラインの仕組みを理解します。

## フロントエンドパイプラインの概要、メリットおよび考慮事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>これは、AEM as a Cloud Service のみを対象としており、AMS ベースの Adobe Cloud Manager のデプロイメントには当てはまりません。

## 前提条件

このチュートリアルのデプロイメントステップは Adobe Cloud Manager で行います。__デプロイメントマネージャー__&#x200B;の役割が付与されていることを確認してください。Cloud Manage の[役割の定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=ja#role-definitions)を参照してください。

このチュートリアルを実施する際は、必ず[サンドボックスプログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=ja)と[開発環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=ja)を使用してください。

## 次の手順 {#next-steps}

ステップバイステップのチュートリアルで、[AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)を変換してフロントエンドパイプラインを有効にする方法を順を追って説明します。

すぐに取りかかりましょう。まずは、[フルスタックプロジェクトのレビュー](review-uifrontend-module.md)の章に移動してチュートリアルを開始し、標準 AEM Sites プロジェクトのコンテキストでのフロントエンド開発ライフサイクルの概要を確認します。
