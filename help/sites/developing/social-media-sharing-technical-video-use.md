---
title: AEM Sitesでのソーシャルメディア共有の使用
description: ソーシャルメディア共有コンポーネントの設定と使用について説明します。
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 10%

---

# ソーシャルメディア共有の使用 {#using-social-media-sharing-in-aem-sites}

ソーシャルメディア共有コンポーネントの設定と使用について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

このビデオでは、ソーシャルメディア共有コンポーネント ( [AEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)) を [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) サンプル Web サイト。

* 0:00 — ソーシャルメディア共有コンポーネントの追加と設定
* 1:00 - Facebookとの共有
* 3:10 - Pinterestとの共有
* 6:25 — 製品ページでのソーシャルメディア共有コンポーネントの使用

## Externalizer の設定 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizer](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) は、AEM オーサーと AEM パブリッシュの両方で設定する必要があります。これにより、パブリッシュ実行モードを、AEM パブリッシュへのアクセスに使用される公開アクセス可能なドメインにマッピングします。

このビデオでは、 `/etc/hosts` 偽物 *www.example.com* localhost に解決し、 [基本的なAEM Dispatcher 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) www.example.comを使用して AEM パブリッシュを前面に表示することを許可します。

## サポート資料 {#supporting-materials}

* [AEMコアコンポーネントのダウンロード](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail をダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher のインストール](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
