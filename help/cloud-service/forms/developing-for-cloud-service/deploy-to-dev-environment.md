---
title: ローカル開発環境へのデプロイ
description: Cloud Manager リポジトリブランチからのコードのデプロイ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 23
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '118'
ht-degree: 100%

---

# ローカル開発環境へのデプロイ

前の手順では、ローカル Git リポジトリから Cloud Manager リポジトリの MyFirstAF ブランチにメインブランチをプッシュしました。

次の手順では、コードを開発環境にデプロイします。
Cloud Manager にログインし、プログラムを選択します。

「開発環境にデプロイ」を選択します（下図を参照）。


![最初の手順](assets/deploy-first-step1.png)


「デプロイメントパイプライン」を選択します（下図を参照）。
![最初の手順](assets/deploy1.png)

ソースコードと適切な Git ブランチを選択します。
![最初の手順](assets/deploy2.png)
必ず変更内容を更新してください。

パイプラインを実行します。
![パイプラインの実行](assets/run-pipeline.png)

コードをデプロイすると、AEM Forms の Cloud Service インスタンスに変更内容が表示されます。

## 次の手順

[Maven アーキタイププロジェクトの更新](./updating-project-archetype.md)
