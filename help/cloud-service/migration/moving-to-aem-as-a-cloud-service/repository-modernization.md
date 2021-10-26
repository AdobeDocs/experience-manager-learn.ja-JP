---
title: リポジトリの最新化
description: リポジトリの最新化、可変および不変コンテンツ、パッケージ構造、およびリポジトリの最新化 CLI ツールについて説明します。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 11%

---

# リポジトリの最新化

リポジトリの最新化、可変および不変コンテンツ、パッケージ構造、およびリポジトリの最新化 CLI ツールについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336958/?quality=12&learn=on)

## Repository Modernizer ツール

![Dispatcher コンバーター](./assets/repository-modernizer.png)

コードベースのリファクタリングの一環として、 [Repository Modernizer ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html?lang=ja) 6.x コードベースをより新しい構造に再構築する場合。

### 重要なアクティビティ

* 使用する [Adobe I/ORepository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) ツールを使用して、AEM as a Cloud Serviceプロジェクトの期待される構造に合うようにプロジェクトを再構築します。
* 更新されたコードベースのビルドエラーを手動で調整して修正します。
* の設定 [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) 更新されたコードベースをデプロイします。 プロジェクトが安定状態になるまで繰り返し実行します。
* 更新されたコードベースをAEMas a Cloud Service開発環境にデプロイし、引き続き検証します。
