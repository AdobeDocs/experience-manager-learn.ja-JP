---
title: AEM Assetsの透かし
description: AEM as a Cloud Serviceの透かし処理機能を使用すると、カスタムの画像レンディションを、任意のPNG画像を使用して透かし処理できます。
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# 透かし

AEM as a Cloud Serviceの透かし処理機能を使用すると、カスタムの画像レンディションを、任意のPNG画像を使用して透かし処理できます。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi設定

次のOSGi設定スタブを更新し、AEMプロジェクトの`ui.config`サブプロジェクトに追加できます。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
