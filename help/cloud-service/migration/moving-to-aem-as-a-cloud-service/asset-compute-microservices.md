---
title: AEM Assets Microservices とAEM as a Cloud Serviceへの移行
description: AEM Assetsas a Cloud Serviceのasset computeマイクロサービスで、従来のAEMワークフローのこの役割に代わって、アセットに関するレンディションを自動的かつ効率的に生成する方法を説明します。
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 9%

---

# AEM Assets Microservices - AEM as a Cloud Serviceへの移行

AEM Assetsas a Cloud Serviceのasset computeマイクロサービスで、従来のAEMワークフローのこの役割に代わって、アセットに関するレンディションを自動的かつ効率的に生成する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## ワークフロー移行ツール

![アセットワークフロー移行ツール](./assets/asset-workflow-migration.png)

コードベースのリファクタリングの一環として、 [アセットワークフロー移行ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=ja) 既存のワークフローを移行して、AEM as a Cloud ServiceのAsset computeマイクロサービスを使用する場合。

### 重要なアクティビティ

* 以下を使用： [Adobe I/OWorkflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) ツールを使用してアセット処理ワークフローを移行し、Asset computeマイクロサービスを使用する
* の設定 [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) 更新されたワークフローをデプロイします。 複雑なワークフローでは、手動の調整が必要になる場合があります。
* 更新されたローカル開発が機能パリティと一致するまで、AEM SDK を使用したワークフロー環境で繰り返し処理を行います。
* 更新されたコードベースをAEMas a Cloud Serviceの開発環境にデプロイし、検証を続行します。

