---
title: リポジトリの最新化
description: リポジトリの最新化、可変コンテンツと不変コンテンツ、パッケージ構造、および repository modernizer CLI ツールについて説明します。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 9%

---

# リポジトリの最新化

リポジトリの最新化、可変コンテンツと不変コンテンツ、パッケージ構造、および repository modernizer CLI ツールについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## Repository Modernizer ツール

![Repository Modernizer](./assets/repository-modernizer.png)

コードベースのリファクタリングの一環として、 [Repository Modernizer ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html?lang=ja) 6.x コードベースをより新しい構造に再構築する場合。

## 主要なアクティビティ

* 以下を使用： [Adobe I/ORepository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) ツールを使用して、AEM as a Cloud Serviceプロジェクトの期待される構造に合わせてプロジェクトを再構築します。
* 更新されたコードベースのビルドエラーを手動で調整し、修正します。
* の設定 [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) 更新されたコードベースをデプロイします。 プロジェクトが安定した状態になるまで繰り返し実行します。
* 更新されたコードベースをAEMas a Cloud Serviceの開発環境にデプロイし、検証を続行します。
