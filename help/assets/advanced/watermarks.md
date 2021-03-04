---
title: AEM Assetsの透かし
description: AEMをCloud Serviceの透かし機能として使用すると、カスタムの画像レンディションを任意のPNG画像を使用して透かし表示できます。
feature: asset computeマイクロサービス
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: コンテンツ管理
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# 透かし

AEMをCloud Serviceの透かし機能として使用すると、カスタムの画像レンディションを任意のPNG画像を使用して透かし表示できます。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi設定

次のOSGi構成スタブを更新し、AEMプロジェクトの`ui.config`サブプロジェクトに追加できます。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
