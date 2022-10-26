---
title: 標準AEMプロジェクトアーキタイプのフロントエンドパイプラインを有効にする
description: CSS、JavaScript、フォント、アイコンなどの静的リソースをより迅速にデプロイするために、標準AEMプロジェクトのフロントエンドパイプラインを有効にする方法を説明します。 また、AEM上のフルスタックバックエンド開発からフロントエンド開発を切り離すこともできます。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# 標準AEMプロジェクトアーキタイプのフロントエンドパイプラインを有効にする{#enable-front-end-pipeline-standard-aem-project}

を有効にする方法を説明します。 [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd) ( 別名 Standard AEM Project) を使用して作成 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) ：フロントエンドパイプラインを使用して CSS、JavaScript、Fonts、Icons などのフロントエンドリソースをデプロイし、開発からデプロイメントまでのサイクルを高速化します。 AEM上のフルスタックバックエンド開発からフロントエンド開発が分離されたこと。 また、これらのフロントエンドリソースがどのように機能するかについても説明します __not__ はAEMリポジトリから提供されますが、CDN から提供されるのは、配信パラダイムの変化です。


ビルドとデプロイのみをおこなう新しいフロントエンドパイプラインが、AdobeCloud Manager で作成されます `ui.frontend` アーティファクトを組み込みの CDN に送信し、AEMにその場所を通知します。 Web ページのHTML生成時に、AEMで `<link>` および `<script>` タグ。 `href` 属性値。

ただし、WKND Sites AEMプロジェクト変換後は、フロントエンド開発者は、独自のデプロイパイプラインを持つAEM上のフルスタックバックエンド開発とは別に、並行して作業をおこなうことができます。

>[!IMPORTANT]
>
>一般に、フロントエンドパイプラインは [AEMクイックサイト作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)、関連するチュートリアルがあります [AEM Sites使用の手引き — クイックサイト作成](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 詳しくは、こちらを参照してください。 このチュートリアルと関連ビデオでは、参考になるものは、微妙な違いを呼び出し、重要な概念を説明する直接的または間接的な比較を行うことを確認することです。


関連 [複数手順のチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) では、クイックサイト作成機能を使用して、架空のライフスタイルブランド向けのAEMサイトを WKND で実装する方法について説明します。 レビュー [テーマ設定ワークフロー](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) フロントエンドパイプラインの動作を理解することも役立ちます。

## フロントエンドパイプラインの概要、メリットおよび考慮事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>これは、AEMas a Cloud Serviceにのみ適用され、AMS ベースの Cloud Manager デプロイメントには適用されません。

## 前提条件

このチュートリアルのデプロイメント手順は、Cloud Manager でAdobeし、 __デプロイメントマネージャー__ の役割（「クラウド管理」を参照） [ロールの定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

必ず [サンドボックスプログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) および [開発環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) このチュートリアルを完了する際

## 次の手順 {#next-steps}

ステップバイステップのチュートリアルでは、 [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd) 変換を使用して、フロントエンドパイプラインに対して有効にすることができます。

何を待ってるの？ チュートリアルを開始するには、 [フルスタックプロジェクトを確認](review-uifrontend-module.md) の章を参照し、標準のAEM Sitesプロジェクトのコンテキストで、フロントエンド開発のライフサイクルをまとめます。

