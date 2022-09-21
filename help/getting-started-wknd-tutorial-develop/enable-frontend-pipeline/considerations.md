---
title: 開発に関する考慮事項
description: フロントエンドパイプラインを有効にした後は、フロントエンドおよびバックエンドの開発プロセスへの影響を考慮します。
sub-product: sites
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
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# 開発に関する考慮事項

フロントエンドパイプラインでAEMas a Cloud Serviceの環境にフロントエンドリソースのみをデプロイできるようにした後は、ローカルのAEMの開発に多少の影響があり、git ブランチモデルを調整する必要があります。

## 目的

* フロントエンドとバックエンドの開発フローが摩擦のない状態になる方法
* フルスタックとフロントエンドパイプラインの依存関係を確認する


## ローカル開発の考慮事項

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## 調整済みの開発アプローチ

* AEM SDK を使用するローカル開発では、バックエンド開発チームは、を介して clientlib を生成する必要があります `ui.frontend` モジュールですが、AEM as a Cloud Service環境に Cloud Manager をデプロイする際は、スキップする必要があります。 ここでは、 [プロジェクトを更新](update-project.md) チャプター。

A __ソリューション__ は、git ブランチモデルを調整し、AEMプロジェクト設定の変更が __ローカル開発__ AEMバックエンド開発者が使用するをブランチします。


* AEMプロジェクトの継続的な強化の一環として、新しいコンポーネントを導入するか、両方に変更がある既存のコンポーネントを更新する場合。 `ui.app` および `ui.frontend` モジュールの場合は、フルスタックとフロントエンドの両方のパイプラインを実行する必要があります。



