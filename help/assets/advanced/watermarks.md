---
title: AEM Assetsの透かし
description: AEM as a Cloud Serviceの透かし処理機能を使用すると、カスタムの画像レンディションを、任意のPNG画像を使用して透かし処理できます。
feature: asset computeマイクロサービス
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: コンテンツ管理
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 3%

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
