---
title: AEM Assetsの透かし
description: AEMをCloud Serviceの透かし機能として使用すると、カスタムの画像レンディションを任意のPNG画像を使用して透かし表示できます。
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# 透かし

AEMをCloud Serviceの透かし機能として使用すると、カスタムの画像レンディションを任意のPNG画像を使用して透かし表示できます。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi設定

次のOSGi構成スタブを更新し、AEMプロジェクトの `ui.config` サブプロジェクトに追加できます。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
