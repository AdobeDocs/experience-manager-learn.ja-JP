---
title: 開発に関する考慮事項
description: フロントエンドパイプラインを有効にすると、フロントエンドおよびバックエンドの開発プロセスに与える影響を考慮します。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '201'
ht-degree: 100%

---

# 開発に関する考慮事項

フロントエンドパイプラインで AEM as a Cloud Service 環境にフロントエンドリソースのみをデプロイできるようにすると、ローカルの AEM 開発に多少の影響があり、Git ブランチモデルを微調整する必要があります。

## 目的

* 摩擦のないフロントエンドとバックエンドの開発フローを実現する方法
* フルスタックとフロントエンドパイプライン間の依存関係の確認


## ローカル開発に関する考慮事項

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 調整済みの開発方法

* AEM SDK を使用したローカル開発の場合、バックエンド開発チームは `ui.frontend` モジュールを介して clientlib を生成する必要がありますが、Cloud Manager を AEM as a Cloud Service 環境にデプロイする間はスキップする必要があります。これにより、[プロジェクトのアップデート](update-project.md)の章で説明しているプロジェクト設定の変更をどのように分離するかという課題が表面化します。

__解決策__&#x200B;は、Git ブランチモデルを調整し、AEM プロジェクト設定の変更が AEM バックエンド開発者が使用する&#x200B;__ローカル開発__&#x200B;ブランチに戻らないようにすることです。


* AEM プロジェクトの継続的な強化の一環として、新しいコンポーネントを導入するか、`ui.app` と `ui.frontend` モジュールの両方に変更がある既存のコンポーネントを更新する場合、フルスタックとフロントエンドパイプラインの両方を実行する必要があります。
