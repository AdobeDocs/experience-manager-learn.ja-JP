---
title: コンテンツ転送ツールを使用したコンテンツ移行
description: コンテンツ転送ツールを使用して、AEM 6 からas a Cloud Serviceにコンテンツを移行する方法を説明します。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 2%

---


# コンテンツ転送ツール

コンテンツ転送ツールを使用して、AEM 6.3 以降からas a Cloud Serviceにコンテンツを移行する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336970/?quality=12&learn=on)

## コンテンツ転送ツールの使用

![コンテンツ転送ツールのライフサイクル](../assets/content-transfer-tool.png)

コンテンツ転送ツールは、AEM 6.3 以降にインストールされ、コンテンツをAEM as a Cloud Serviceに転送します。

### 重要なアクティビティ

+ をダウンロードします。 [最新のコンテンツ転送ツール](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=2).
+ AEM Author 6.3 以降の最終コンテンツをAEMas a Cloud Serviceオーサーサービスに転送します。
   + 転送する最終コンテンツを含むAEM 6.3 以降のオーサーにコンテンツ転送ツールをインストールします。
   + コンテンツ転送ツールをバッチで実行し、コンテンツのセットを転送します。
+ AEM Publish 6.3 以降の最終コンテンツをAEM as a Cloud Service Publish Service に転送します。
   + 転送する最終コンテンツを含むAEM 6.3 以降の Publish にコンテンツ転送ツールをインストールします。
   + コンテンツ転送ツールをバッチで実行し、コンテンツのセットを転送します。
+ （オプション）最後のコンテンツ転送以降に新しいコンテンツを転送して、AEMas a Cloud Service上の「追加」コンテンツを作成する

### Adobe  に関するその他のリソース

+ [コンテンツ転送ツールをダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [一括読み込みサービスのハウツービデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html?lang=en)
