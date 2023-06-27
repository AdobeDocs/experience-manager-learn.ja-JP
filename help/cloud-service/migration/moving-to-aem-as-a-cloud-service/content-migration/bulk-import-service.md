---
title: 一括読み込みサービスを使用したコンテンツの移行
description: AEM as a Cloud Service の一括読み込みサービスを使用して、AEM 以外のソースからアセットを読み込む方法について説明します。
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
kt: 8918
thumbnail: 336969.jpeg
exl-id: 4944d3d9-52a0-4255-9e6c-eb119160e400
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 100%

---

# 一括読み込みサービス

AEM as a Cloud Service の一括読み込みサービスを使用して、AEM 以外のソースからアセットを読み込む方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336969?quality=12&learn=on)

## 一括読み込みサービスの使用

![一括読み込みサービスのライフサイクル](../assets/bulk-import-service.png)

一括読み込みサービスは、Azure Blob ストレージまたは Amazon S3 ストレージに保存されたファイルをアセットとして AEM as a Cloud Service に転送するために使用されます。

## 重要なアクティビティ

+ 読み込むファイルをクラウドストレージプロバイダー（Azure Blob ストレージまたは Amazon S3） にアップロードします。
+ AEM as a Cloud Service オーサーサービスを設定して、一括読み込みサービスを実行します。
+ 一括サービスインポーターを 1 回限りの読み込みとして実行するか、定期的な読み込みスケジュールを設定します。

## その他のリソース

+ [一括読み込みサービス設定オプション](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/add-assets.html?lang=ja#configure-bulk-ingestor-tool)
+ [アセット取り込みに関する Adobe Developers Live セッション](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/feb2021/asset-bulk-ingestion.html?lang=ja)

