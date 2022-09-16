---
title: 標準AEMプロジェクトアーキタイプのフロントエンドパイプラインの有効化
description: 標準のAEMプロジェクトを変換して、フロントエンドパイプラインを使用した CSS、JavaScript、フォント、アイコンなどの静的リソースのデプロイを高速化します。 AEM上のフルスタックバックエンド開発からフロントエンド開発を分離する方法。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---


# 標準AEMプロジェクトアーキタイプのフロントエンドパイプラインの有効化{#enable-front-end-pipeline-standard-aem-project}

を使用して作成された標準AEMプロジェクトを有効にする方法を説明します。 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) ：フロントエンドパイプラインを使用して CSS、JavaScript、フォント、アイコンなどの静的リソースをデプロイし、開発からデプロイメントまでのサイクルを迅速に実行する場合。 AEM上のフルスタックバックエンド開発からフロントエンド開発を分離する方法。 また、これらの静的リソースの機能についても学習します __not__ はAEMリポジトリから提供されますが、CDN から提供されるのは、配信パラダイムの変化です。

ビルドとデプロイのみをおこなう新しいフロントエンドパイプラインがAdobeCloud Manager で作成されます `ui.frontend` アーティファクトを CDN に送信し、その場所をAEMに通知します。 Web ページのHTML生成時に、 `<link>` および `<script>` タグは、 `href` 属性値。

標準のAEMプロジェクト変換後、フロントエンド開発者は、独自のデプロイパイプラインを持つAEM上のフルスタックバックエンド開発とは別に、並行して作業をおこなうことができます。

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>これはAEMas a Cloud Serviceにのみ適用され、AMS ベースの Cloud Manager デプロイメントには適用されません。

