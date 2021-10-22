---
title: 開発環境へのデプロイ
description: Cloud Manager リポジトリブランチからコードをデプロイします。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---


# 開発環境へのデプロイ

前の手順では、ローカル Git リポジトリーから Cloud Manager リポジトリーの MyFirstAF ブランチに master ブランチをプッシュしました。

次の手順では、コードを開発環境にデプロイします。
Cloud Manager にログインし、プログラムを選択します。

次に示すように、「開発環境にデプロイ」を選択します。


![最初の段階](assets/deploy-first-step1.png)


図のように、デプロイメントパイプラインを選択します
![最初の段階](assets/deploy1.png)

ソースコードと適切な Git ブランチを選択します。
![最初の段階](assets/deploy2.png)
必ず変更を更新してください

パイプラインの実行
![実行パイプライン](assets/run-pipeline.png)

コードをデプロイすると、AEM Formsのクラウドサービスインスタンスに変更が表示されます。
