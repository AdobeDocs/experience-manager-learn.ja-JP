---
title: AEM as a Cloud Serviceでの検索とインデックス作成
description: AEMas a Cloud Serviceの検索インデックス、AEM 6 のインデックス定義の変換方法、およびインデックスのデプロイ方法について説明します。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 4%

---

# 検索とインデックス作成

AEMas a Cloud Serviceの検索インデックス、AEM 6 のインデックス定義をAEMにas a Cloud Service的な互換性を持たせる方法、およびインデックスをAEM as a Cloud Serviceにデプロイする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## インデックスコンバータツール

![インデックスコンバータツール](./assets/index-converter.png)

コードベースのリファクタリングの一環として、 [インデックスコンバーターツール](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) カスタムの Oak インデックス定義をAEMas a Cloud Serviceの互換性のあるインデックス定義に変換する場合。

### 重要なアクティビティ

* 以下を使用： [Adobe I/OWorkflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ツールを使用してアセット処理ワークフローを移行し、Asset computeマイクロサービスを使用する
* の設定 [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) カスタマイズされたインデックスをデプロイします。 更新されたインデックスが最新であることを確認します。
* 更新されたコードベースをAEMas a Cloud Serviceの開発環境にデプロイし、検証を続行します。
* 標準提供のインデックスを変更する場合 **常に** 最新のリリースで実行されているAEMas a Cloud Service環境から最新のインデックス定義をコピーします。 必要に応じて、コピーしたインデックス定義を変更します。