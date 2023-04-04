---
title: 一括読み込みサービスを使用したコンテンツの移行
description: AEM as a AEM Bulk Import Service を使用して、非Cloud Servicesソースからアセットを読み込む方法を説明します。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8918
thumbnail: 336969.jpeg
exl-id: 4944d3d9-52a0-4255-9e6c-eb119160e400
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 1%

---

# 一括読み込みサービス

AEM as a AEM Bulk Import Service を使用して、非Cloud Servicesソースからアセットを読み込む方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336969?quality=12&learn=on)

## 一括読み込みサービスの使用

![一括インポートサービスのライフサイクル](../assets/bulk-import-service.png)

一括読み込みサービスは、Azure Blob ストレージまたはAmazon S3 ストレージに保存されたファイルを、アセットとしてAEM as a Cloud Serviceに転送するために使用されます。

## 主要なアクティビティ

+ インポートするファイルをクラウドストレージプロバイダー (Azure Blob ストレージまたはAmazon S3) にアップロードします。
+ AEM as a Cloud Service Author サービスを設定して、一括読み込みサービスを実行します。
+ 一括サービスインポーターを 1 回限りのインポートとして実行するか、定期的なインポートのスケジュールを設定します。

## その他のリソース

+ [Adobe Developers Liveセッション（アセット取り込み時）](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/feb2021/asset-bulk-ingestion.html?lang=en)

