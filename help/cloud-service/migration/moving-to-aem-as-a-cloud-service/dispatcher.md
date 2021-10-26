---
title: AEM as a Cloud Serviceへの移行時の Dispatcher の設定
description: AEM as a Cloud Service向けAEM Dispatcher の主な変更点、Dispatcher 変換ツール、および Dispatcher ツール SDK の使用方法について説明します。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 4%

---

# Dispatcher

AEM 6 向け Dispatcher の主な変更点、Dispatcher 変換ツール、および Dispatcher ツール SDK の使用方法に重点を置いた、AEMas a Cloud Service版のAEM Dispatcher について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher コンバーター

![Dispatcher コンバーター](./assets/dispatcher-converter-diagram.png)

コードベースのリファクタリングの一環として、 [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 既存のオンプレミス設定または Adobe Managed Services Dispatcher 設定を、AEMとas a Cloud Serviceの互換性のある Dispatcher 設定にリファクタリングする場合。

### 重要なアクティビティ

* 使用する [Adobe I/ODispatcher コンバーターツール](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 既存の Dispatcher 設定を移行する場合。
* からの Dispatcher モジュールの参照 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) ベストプラクティスとして。
* [ローカル Dispatcher ツールの設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) dispatcher 環境でテストする前に、dispatcher をCloud Serviceする。


