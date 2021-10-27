---
title: AEM as a Cloud Service のオンボーディング
description: Cloud Manager を使用した環境の設定に至るまで、契約段階から始めて、AEMas a Cloud Serviceのオンボーディングについて説明します。
version: Cloud Service
feature: Onboarding
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8631
thumbnail: 336959.jpeg
exl-id: 9d2004e5-e928-4190-8298-695635c8e92c
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 15%

---

# AEM as a Cloud Service のオンボーディング

Cloud Manager を使用した環境の設定まで、契約段階から始まり、AEMas a Cloud Serviceのオンボーディングについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336959/?quality=12&learn=on)

## Cloud Manager と Admin Console

![概要図のオンボーディング](assets/onboarding-diagram.png)

オンボーディングの重要な部分は、AEMas a Cloud Serviceのプログラムを作成し、Cloud Manager を使用して様々な環境をプロビジョニングすることです。 この [Admin Console](https://adminconsole.adobe.com/) は、役割を割り当て、組織内のユーザーにAEM環境へのアクセスを提供するために使用します。

### 重要なアクティビティ

* システム管理者は、 [Admin Console](https://adminconsole.adobe.com/) 1 人以上のユーザーを [Cloud Manager — ビジネスオーナー](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html) 製品プロファイル。
* ビジネスオーナー製品プロファイルに割り当てられたユーザーは、 [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=ja) から [プログラムを作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/production-programs/creating-production-program.html) および [環境の追加](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=ja)
* 以下を使用： [Admin Console](https://adminconsole.adobe.com/) 開発者とユーザーを別の [Cloud Manager のロール](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html) 様々なAEM環境に権限を付与できます。